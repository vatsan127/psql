# PostgreSQL Commands Reference

## pg Command Line Tools

### pg_isready
Check if server is accepting connections.
```bash
pg_isready
pg_isready -h localhost -p 5432
```

### createdb
Create a new database.
```bash
createdb -U srivatsan mydb
createdb -U srivatsan -O srivatsan -E UTF8 mydb
```

### dropdb
Drop a database.
```bash
dropdb -U srivatsan mydb
```

### pg_dump
Backup a database.
```bash
pg_dump -U srivatsan mydb > backup.sql
pg_dump -U srivatsan -Fc mydb > backup.dump
```

### pg_restore
Restore a database from backup.
```bash
pg_restore -U srivatsan -d mydb backup.dump
pg_restore -d postgresql://srivatsan:password@localhost:5432/mydb -Fc backup.dump -c -v
```

### pg_dumpall
Backup all databases.
```bash
pg_dumpall -U postgres > all_databases.sql
```

---

## psql Meta-Commands

### Connection

| Command | Description |
|---------|-------------|
| `\c dbname` | Connect to database |
| `\c dbname user` | Connect as user to database |
| `\conninfo` | Show current connection info |
| `\q` | Quit psql |
| `SET search_path TO schema` | Change current schema |

### Listing Objects

| Command | Description |
|---------|-------------|
| `\l` | List all databases |
| `\l+` | List databases with size and description |
| `\dn` | List schemas |
| `\dn+` | List schemas with permissions |
| `\dt` | List tables in current schema |
| `\dt+` | List tables with size and description |
| `\dt schema.*` | List tables in specific schema |
| `\di` | List indexes |
| `\dv` | List views |
| `\dm` | List materialized views |
| `\ds` | List sequences |
| `\df` | List functions |
| `\du` | List roles/users |

### Describe Objects

| Command | Description |
|---------|-------------|
| `\d tablename` | Describe table structure |
| `\d+ tablename` | Describe table with extra details |
| `\d schema.table` | Describe table in specific schema |

### Query Buffer

| Command | Description |
|---------|-------------|
| `\e` | Edit query in external editor |
| `\g` | Execute last query |
| `\p` | Print current query buffer |
| `\r` | Reset query buffer |

### Input/Output

| Command | Description |
|---------|-------------|
| `\i filename` | Execute commands from file |
| `\o filename` | Send output to file |
| `\o` | Stop sending output to file |
| `\copy` | Copy data between file and table |

### Formatting

| Command | Description |
|---------|-------------|
| `\x` | Toggle expanded display |
| `\x on` | Enable expanded display |
| `\x off` | Disable expanded display |
| `\timing` | Toggle query timing |
| `\pset border 2` | Set table border style |

### Help

| Command | Description |
|---------|-------------|
| `\?` | Help on psql commands |
| `\h` | Help on SQL commands |
| `\h SELECT` | Help on specific SQL command |

### Variables

| Command | Description |
|---------|-------------|
| `\set` | Show all variables |
| `\set VAR value` | Set variable |
| `\unset VAR` | Unset variable |
| `\echo :VAR` | Print variable value |
