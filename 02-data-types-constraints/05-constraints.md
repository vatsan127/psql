# Constraints

## Overview

| Constraint | Purpose |
|------------|---------|
| `PRIMARY KEY` | Unique identifier for each row |
| `FOREIGN KEY` | References another table |
| `UNIQUE` | No duplicate values |
| `NOT NULL` | Cannot be null |
| `CHECK` | Custom validation |
| `DEFAULT` | Default value if not provided |

## PRIMARY KEY

Unique + NOT NULL. One per table.

```sql
-- Single column
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100)
);

-- Composite primary key
CREATE TABLE order_items (
    order_id INTEGER,
    product_id INTEGER,
    quantity INTEGER,
    PRIMARY KEY (order_id, product_id)
);

-- Named constraint
CREATE TABLE products (
    id SERIAL,
    CONSTRAINT pk_products PRIMARY KEY (id)
);
```

## FOREIGN KEY

References primary key in another table.

```sql
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id),
    total NUMERIC(10, 2)
);

-- With explicit constraint name and actions
CREATE TABLE order_items (
    id SERIAL PRIMARY KEY,
    order_id INTEGER,
    product_id INTEGER,
    CONSTRAINT fk_order
        FOREIGN KEY (order_id)
        REFERENCES orders(id)
        ON DELETE CASCADE
        ON UPDATE CASCADE
);
```

### ON DELETE / ON UPDATE Actions

| Action | Behavior |
|--------|----------|
| `CASCADE` | Delete/update child rows too |
| `SET NULL` | Set foreign key to NULL |
| `SET DEFAULT` | Set to default value |
| `RESTRICT` | Prevent delete/update (default) |
| `NO ACTION` | Same as RESTRICT |

```sql
-- If user is deleted, delete their orders too
user_id INTEGER REFERENCES users(id) ON DELETE CASCADE

-- If user is deleted, set to NULL
user_id INTEGER REFERENCES users(id) ON DELETE SET NULL
```

## UNIQUE

No duplicate values allowed (NULLs are allowed and don't count as duplicates).

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE,
    username VARCHAR(50) UNIQUE
);

-- Composite unique (combination must be unique)
CREATE TABLE user_roles (
    user_id INTEGER,
    role_id INTEGER,
    UNIQUE (user_id, role_id)
);

-- Named constraint
CREATE TABLE products (
    sku VARCHAR(50),
    CONSTRAINT uq_sku UNIQUE (sku)
);
```

## NOT NULL

Column cannot contain NULL.

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) NOT NULL,
    name VARCHAR(100) NOT NULL
);
```

## CHECK

Custom validation rule.

```sql
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    price NUMERIC(10, 2) CHECK (price > 0),
    quantity INTEGER CHECK (quantity >= 0)
);

-- Named check constraint
CREATE TABLE users (
    age INTEGER,
    CONSTRAINT chk_age CHECK (age >= 18 AND age <= 120)
);

-- Multiple column check
CREATE TABLE events (
    start_date DATE,
    end_date DATE,
    CONSTRAINT chk_dates CHECK (end_date >= start_date)
);
```

## DEFAULT

Value used when not provided.

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    is_active BOOLEAN DEFAULT TRUE,
    role VARCHAR(20) DEFAULT 'user',
    score INTEGER DEFAULT 0
);

INSERT INTO users DEFAULT VALUES;  -- All defaults used
INSERT INTO users (role) VALUES ('admin');  -- Other columns get defaults
```

## Adding/Dropping Constraints

```sql
-- Add constraint to existing table
ALTER TABLE users ADD CONSTRAINT uq_email UNIQUE (email);
ALTER TABLE users ADD CONSTRAINT chk_age CHECK (age >= 0);
ALTER TABLE orders ADD CONSTRAINT fk_user FOREIGN KEY (user_id) REFERENCES users(id);

-- Add NOT NULL
ALTER TABLE users ALTER COLUMN email SET NOT NULL;

-- Drop constraint
ALTER TABLE users DROP CONSTRAINT uq_email;
ALTER TABLE users DROP CONSTRAINT chk_age;

-- Drop NOT NULL
ALTER TABLE users ALTER COLUMN email DROP NOT NULL;
```

## Combining Constraints

```sql
CREATE TABLE employees (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) NOT NULL UNIQUE,
    salary NUMERIC(10, 2) NOT NULL CHECK (salary > 0),
    department_id INTEGER NOT NULL REFERENCES departments(id),
    created_at TIMESTAMPTZ DEFAULT NOW() NOT NULL
);
```
