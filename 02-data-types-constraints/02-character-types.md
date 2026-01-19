# Character Types

## Types Overview

| Type | Description |
|------|-------------|
| `CHAR(n)` | Fixed-length, blank-padded |
| `VARCHAR(n)` | Variable-length with limit |
| `TEXT` | Variable-length, unlimited |

## CHAR(n) - Fixed Length

- Always stores exactly `n` characters
- Pads with spaces if input is shorter
- Rarely used in practice

```sql
CREATE TABLE example (
    code CHAR(5)
);

INSERT INTO example VALUES ('AB');
SELECT code, LENGTH(code) FROM example;
-- Result: 'AB   ', length = 5 (padded with spaces)
```

## VARCHAR(n) - Variable Length with Limit

- Stores up to `n` characters
- No padding
- Most commonly used

```sql
CREATE TABLE users (
    name VARCHAR(100),
    email VARCHAR(255)
);

INSERT INTO users VALUES ('John', 'john@example.com');
-- Stores exactly what you insert, no padding
```

## TEXT - Unlimited Length

- No length limit
- Same performance as VARCHAR
- Use when length is unpredictable

```sql
CREATE TABLE posts (
    title VARCHAR(200),
    content TEXT
);
```

## Comparison

| Type | Max Length | Padding | Use Case |
|------|------------|---------|----------|
| `CHAR(n)` | n | Yes | Fixed codes (country, currency) |
| `VARCHAR(n)` | n | No | Most string data |
| `TEXT` | Unlimited | No | Long content, unknown length |

## Performance Note

In PostgreSQL, there's no performance difference between VARCHAR(n) and TEXT. The length limit in VARCHAR is only for validation.

```sql
-- These perform the same:
name VARCHAR(100)
name TEXT CHECK (LENGTH(name) <= 100)
```

## Common Lengths

| Data | Suggested Type |
|------|----------------|
| Name | VARCHAR(100) |
| Email | VARCHAR(255) |
| Phone | VARCHAR(20) |
| Country code | CHAR(2) |
| UUID | CHAR(36) or UUID type |
| Description | TEXT |
