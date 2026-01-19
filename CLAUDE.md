# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Purpose

This is a PostgreSQL learning project with a sample employees database for practicing SQL queries.

## Database Setup

Connect to PostgreSQL:
```bash
psql -U srivatsan -d employees
```

Restore the sample database (if needed):
```bash
pg_restore -d postgresql://srivatsan:password@localhost:5432/employees -Fc employees.sql.gz -c -v --no-owner --no-privileges
```

## Database Schema

- Database: `employees`
- Schema: `employees`
- Key tables: `employees.salary`, `employees.department_employee`, `employees.department`

## Sample Query

```sql
SELECT d.dept_name, AVG(s.amount) AS average_salary
FROM employees.salary s
JOIN employees.department_employee de ON s.employee_id = de.employee_id
JOIN employees.department d ON de.department_id = d.id
WHERE s.to_date > CURRENT_DATE AND de.to_date > CURRENT_DATE
GROUP BY d.dept_name
ORDER BY average_salary DESC
LIMIT 5;
```

---

## Notes Structure

- Each module has its own folder (e.g., `01-basics/`)
- Each topic has a separate file with proper naming
- **pg commands** and **psql meta-commands** go in `commands.md`
- Topic notes should only contain SQL commands
- No "Practice" or "Key Takeaways" sections in notes

---

## Learning Roadmap

### Module 1: Basics
- [x] PostgreSQL architecture (client-server model) → `01-basics/01-postgresql-architecture.md`
- [x] Connecting to databases (`psql` commands) → `01-basics/02-connecting-to-databases.md`
- [x] Creating/dropping databases and schemas → `01-basics/03-creating-dropping-databases-schemas.md`
- [ ] Basic `psql` meta-commands (`\l`, `\dt`, `\d`, `\c`, `\q`)

### Module 2: Data Types & Constraints
- [x] Numeric types (INTEGER, BIGINT, DECIMAL, NUMERIC, REAL, FLOAT) → `02-data-types-constraints/01-numeric-types.md`
- [x] Character types (CHAR, VARCHAR, TEXT) → `02-data-types-constraints/02-character-types.md`
- [x] Date/Time types (DATE, TIME, TIMESTAMP, INTERVAL) → `02-data-types-constraints/03-datetime-types.md`
- [x] Boolean, UUID, Arrays → `02-data-types-constraints/04-other-types.md`
- [x] Constraints (PRIMARY KEY, FOREIGN KEY, UNIQUE, NOT NULL, CHECK, DEFAULT) → `02-data-types-constraints/05-constraints.md`

### Module 3: CRUD Operations
- [ ] INSERT (single, multiple, RETURNING)
- [ ] SELECT basics
- [ ] UPDATE with conditions
- [ ] DELETE vs TRUNCATE
- [ ] UPSERT (INSERT ... ON CONFLICT)

### Module 4: Querying Data
- [ ] WHERE clause and operators
- [ ] ORDER BY, LIMIT, OFFSET
- [ ] DISTINCT and DISTINCT ON
- [ ] LIKE, ILIKE, and pattern matching
- [ ] IN, BETWEEN, IS NULL
- [ ] CASE expressions

### Module 5: Joins & Relationships
- [ ] INNER JOIN
- [ ] LEFT/RIGHT/FULL OUTER JOIN
- [ ] CROSS JOIN
- [ ] Self joins
- [ ] Multiple table joins
- [ ] USING vs ON clause

### Module 6: Aggregations & Grouping
- [ ] COUNT, SUM, AVG, MIN, MAX
- [ ] GROUP BY
- [ ] HAVING clause
- [ ] FILTER clause
- [ ] GROUPING SETS, CUBE, ROLLUP

### Module 7: Subqueries & CTEs
- [ ] Scalar subqueries
- [ ] Correlated subqueries
- [ ] EXISTS, NOT EXISTS
- [ ] ANY, ALL operators
- [ ] Common Table Expressions (WITH clause)
- [ ] Recursive CTEs

### Module 8: Indexes & Performance
- [ ] B-tree indexes (default)
- [ ] Hash, GiST, GIN, BRIN indexes
- [ ] Partial and expression indexes
- [ ] Multi-column indexes
- [ ] Primary vs Secondary indexes
- [ ] CLUSTER (physical ordering by index)
- [ ] EXPLAIN and EXPLAIN ANALYZE
- [ ] Query optimization basics

### Module 9: Views & Materialized Views
- [ ] Creating and using views
- [ ] Updatable views
- [ ] Materialized views
- [ ] REFRESH MATERIALIZED VIEW

### Module 10: Transactions & Concurrency
- [ ] BEGIN, COMMIT, ROLLBACK
- [ ] SAVEPOINT
- [ ] Transaction isolation levels
- [ ] Locking (row-level, table-level)
- [ ] MVCC (Multi-Version Concurrency Control)

### Module 11: Functions & Stored Procedures
- [ ] SQL functions
- [ ] PL/pgSQL basics
- [ ] Function parameters (IN, OUT, INOUT)
- [ ] RETURNS TABLE
- [ ] Stored procedures (CALL)
- [ ] Error handling (EXCEPTION)

### Module 12: Triggers
- [ ] BEFORE/AFTER triggers
- [ ] ROW/STATEMENT level triggers
- [ ] Trigger functions
- [ ] Event triggers

### Module 13: JSON & Advanced Data Types
- [ ] JSON vs JSONB
- [ ] JSON operators (`->`, `->>`, `#>`, `@>`)
- [ ] JSON functions (jsonb_build_object, jsonb_agg)
- [ ] Arrays and array functions
- [ ] ENUM types
- [ ] Composite types

### Module 14: Window Functions
- [ ] ROW_NUMBER, RANK, DENSE_RANK
- [ ] LEAD, LAG
- [ ] FIRST_VALUE, LAST_VALUE, NTH_VALUE
- [ ] SUM/AVG OVER (PARTITION BY)
- [ ] Window frames (ROWS BETWEEN, RANGE BETWEEN)

### Module 15: Administration & Security
- [ ] Users and roles
- [ ] GRANT and REVOKE
- [ ] Row-level security (RLS)
- [ ] Backup (pg_dump, pg_dumpall)
- [ ] Restore (pg_restore, psql)
- [ ] Configuration basics (postgresql.conf)

---

## Progress Notes

*Add notes here as you learn each topic*
