# Connecting to Databases

## Overview

`psql` is PostgreSQL's interactive terminal. It's the primary command-line tool for interacting with PostgreSQL databases.

## Connection Methods

### 1. Basic Connection

```bash
# Connect to default database (same as username)
psql

# Connect to specific database
psql -d employees

# Connect with username
psql -U srivatsan -d employees

# Connect to specific host and port
psql -h localhost -p 5432 -U srivatsan -d employees
```

### 2. Connection URI Format

```bash
# Format: postgresql://user:password@host:port/database
psql postgresql://srivatsan:password@localhost:5432/employees

# With SSL
psql "postgresql://user:pass@host:5432/db?sslmode=require"
```

## Connection Options

| Option | Long Form | Description |
|--------|-----------|-------------|
| `-h` | `--host` | Database server host |
| `-p` | `--port` | Database server port (default: 5432) |
| `-U` | `--username` | Database username |
| `-d` | `--dbname` | Database name |
| `-W` | `--password` | Force password prompt |
| `-w` | `--no-password` | Never prompt for password |
| `-c` | `--command` | Run a single command and exit |
| `-f` | `--file` | Execute commands from a file |

## Running Commands Directly

```bash
# Run a single SQL command
psql -U srivatsan -d employees -c "SELECT current_date;"

# Run multiple commands
psql -U srivatsan -d employees -c "SELECT current_user;" -c "SELECT current_database;"

# Execute SQL from a file
psql -U srivatsan -d employees -f script.sql
```

## Inside psql

### Connection Information

```sql
-- Show current user
SELECT current_user;

-- Show current database
SELECT current_database();

-- Show server version
SELECT version();
```
