# Numeric Types

## Integer Types

| Type | Size | Range |
|------|------|-------|
| `SMALLINT` | 2 bytes | -32,768 to 32,767 |
| `INTEGER` / `INT` | 4 bytes | -2,147,483,648 to 2,147,483,647 |
| `BIGINT` | 8 bytes | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 |

```sql
CREATE TABLE example (
    small_val SMALLINT,
    normal_val INTEGER,
    big_val BIGINT
);
```

## Serial Types (Auto-increment)

| Type | Underlying Type |
|------|-----------------|
| `SMALLSERIAL` | SMALLINT |
| `SERIAL` | INTEGER |
| `BIGSERIAL` | BIGINT |

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100)
);

-- SERIAL is shorthand for:
-- id INTEGER NOT NULL DEFAULT nextval('users_id_seq')
```

## Decimal/Numeric Types (Exact)

| Type | Description |
|------|-------------|
| `NUMERIC(precision, scale)` | Exact, user-specified precision |
| `DECIMAL(precision, scale)` | Same as NUMERIC |

- **precision**: total number of digits
- **scale**: digits after decimal point

```sql
CREATE TABLE products (
    price NUMERIC(10, 2),      -- up to 99999999.99
    tax_rate DECIMAL(5, 4)     -- up to 9.9999
);

-- Without precision: unlimited
CREATE TABLE precise (
    value NUMERIC              -- any precision
);
```

## Floating-Point Types (Approximate)

| Type | Size | Precision |
|------|------|-----------|
| `REAL` | 4 bytes | 6 decimal digits |
| `DOUBLE PRECISION` | 8 bytes | 15 decimal digits |

```sql
CREATE TABLE measurements (
    temp REAL,
    precise_temp DOUBLE PRECISION
);
```

**Warning**: Floating-point types are approximate. Use NUMERIC for money or exact values.

```sql
-- Floating-point precision issue
SELECT 0.1::REAL + 0.2::REAL;  -- May not equal exactly 0.3

-- Use NUMERIC for exact math
SELECT 0.1::NUMERIC + 0.2::NUMERIC;  -- Exactly 0.3
```

## When to Use What

| Use Case | Type |
|----------|------|
| IDs, counts | INTEGER, BIGINT |
| Auto-increment IDs | SERIAL, BIGSERIAL |
| Money, financial | NUMERIC(precision, scale) |
| Scientific calculations | DOUBLE PRECISION |
| Storage-constrained | SMALLINT, REAL |
