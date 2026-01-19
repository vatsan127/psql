# Creating/Dropping Databases and Schemas

## Databases

### Creating Databases

```sql
-- Basic syntax
CREATE DATABASE mydb;

-- With options
CREATE DATABASE mydb
    OWNER = srivatsan
    ENCODING = 'UTF8'
    LC_COLLATE = 'en_US.UTF-8'
    LC_CTYPE = 'en_US.UTF-8'
    TEMPLATE = template0
    CONNECTION LIMIT = 100;
```

### Listing Databases

```sql
SELECT datname FROM pg_database;
```

### Dropping Databases

```sql
-- Basic syntax (cannot drop while connected to it)
DROP DATABASE mydb;

-- Drop if exists (no error if missing)
DROP DATABASE IF EXISTS mydb;

-- Force drop (terminates active connections) - PostgreSQL 13+
DROP DATABASE mydb WITH (FORCE);
```

---

## Schemas

Schemas are namespaces within a database. They help organize tables and avoid naming conflicts.

```
Database
├── Schema: public (default)
│   ├── users
│   └── orders
├── Schema: employees
│   ├── salary
│   └── department
└── Schema: archive
    └── old_orders
```

### Creating Schemas

```sql
-- Basic syntax
CREATE SCHEMA myschema;

-- With owner
CREATE SCHEMA myschema AUTHORIZATION srivatsan;

-- Create if not exists
CREATE SCHEMA IF NOT EXISTS myschema;
```

### Listing Schemas

```sql
SELECT schema_name FROM information_schema.schemata;
```

### Using Schemas

```sql
-- Access table in a schema
SELECT * FROM employees.salary;

-- Set search path (default schema for queries)
SET search_path TO employees, public;

-- Check current search path
SHOW search_path;

-- Create table in specific schema
CREATE TABLE myschema.users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100)
);
```

### Dropping Schemas

```sql
-- Drop empty schema
DROP SCHEMA myschema;

-- Drop if exists
DROP SCHEMA IF EXISTS myschema;

-- Drop schema and all its objects (tables, views, etc.)
DROP SCHEMA myschema CASCADE;
```
