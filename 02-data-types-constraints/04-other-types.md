# Boolean, UUID, and Arrays

## Boolean

| Value | Accepted Inputs |
|-------|-----------------|
| TRUE | `TRUE`, `'t'`, `'true'`, `'yes'`, `'on'`, `1` |
| FALSE | `FALSE`, `'f'`, `'false'`, `'no'`, `'off'`, `0` |
| NULL | `NULL` |

```sql
CREATE TABLE users (
    is_active BOOLEAN DEFAULT TRUE,
    is_verified BOOLEAN DEFAULT FALSE
);

INSERT INTO users (is_active, is_verified) VALUES (TRUE, FALSE);
INSERT INTO users (is_active, is_verified) VALUES ('yes', 'no');

SELECT * FROM users WHERE is_active;
SELECT * FROM users WHERE NOT is_verified;
SELECT * FROM users WHERE is_active IS NOT NULL;
```

## UUID

Universally Unique Identifier - 128-bit value.

```sql
-- Enable uuid extension (if needed)
CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

CREATE TABLE sessions (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    user_id INTEGER
);

INSERT INTO sessions (user_id) VALUES (1);
-- id is auto-generated: 'a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11'

-- PostgreSQL 13+ has built-in gen_random_uuid()
CREATE TABLE tokens (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid()
);
```

## Arrays

PostgreSQL supports arrays of any data type.

```sql
-- Define array columns
CREATE TABLE posts (
    id SERIAL PRIMARY KEY,
    title VARCHAR(200),
    tags TEXT[],
    scores INTEGER[]
);

-- Insert arrays
INSERT INTO posts (title, tags, scores)
VALUES ('PostgreSQL Guide', ARRAY['sql', 'database', 'postgres'], ARRAY[95, 87, 92]);

-- Alternative syntax
INSERT INTO posts (title, tags, scores)
VALUES ('SQL Tips', '{"sql", "tips"}', '{88, 90}');
```

### Array Access

```sql
-- Access by index (1-based)
SELECT tags[1] FROM posts;  -- First element

-- Slice
SELECT tags[1:2] FROM posts;  -- First two elements

-- Array length
SELECT array_length(tags, 1) FROM posts;
```

### Array Operators

```sql
-- Contains
SELECT * FROM posts WHERE tags @> ARRAY['sql'];  -- Has 'sql'
SELECT * FROM posts WHERE 'sql' = ANY(tags);     -- Same as above

-- Contained by
SELECT * FROM posts WHERE tags <@ ARRAY['sql', 'database', 'postgres', 'tips'];

-- Overlap (any common element)
SELECT * FROM posts WHERE tags && ARRAY['sql', 'python'];

-- Concatenate
SELECT tags || ARRAY['new_tag'] FROM posts;
```

### Array Functions

```sql
-- Unnest (expand to rows)
SELECT unnest(tags) FROM posts;

-- Aggregate to array
SELECT array_agg(name) FROM users;

-- Check if contains
SELECT * FROM posts WHERE 'sql' = ANY(tags);
SELECT * FROM posts WHERE 'sql' = ALL(tags);  -- All elements equal 'sql'

-- Array position
SELECT array_position(tags, 'sql') FROM posts;

-- Remove element
SELECT array_remove(tags, 'sql') FROM posts;
```

## JSONB (Brief)

For storing JSON data. Covered in detail in Module 13.

```sql
CREATE TABLE api_logs (
    id SERIAL PRIMARY KEY,
    payload JSONB
);

INSERT INTO api_logs (payload) VALUES ('{"user": "john", "action": "login"}');
SELECT payload->>'user' FROM api_logs;  -- Extract as text
```
