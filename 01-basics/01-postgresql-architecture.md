# PostgreSQL Architecture (Client-Server Model)

## Overview

PostgreSQL uses a **client-server architecture** where multiple clients can connect to a single database server.

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Client 1  │     │   Client 2  │     │   Client 3  │
│   (psql)    │     │   (App)     │     │   (pgAdmin) │
└──────┬──────┘     └──────┬──────┘     └──────┬──────┘
       │                   │                   │
       └───────────────────┼───────────────────┘
                           │
                           ▼
                 ┌─────────────────┐
                 │   PostgreSQL    │
                 │     Server      │
                 │   (postmaster)  │
                 └────────┬────────┘
                          │
                          ▼
                 ┌─────────────────┐
                 │    Database     │
                 │    Cluster      │
                 └─────────────────┘
```

## Key Components

### 1. Postmaster (Main Server Process)
- The main PostgreSQL daemon process
- Listens for incoming connections (default port: 5432)
- Spawns a new **backend process** for each client connection
- Manages shared memory and background processes

### 2. Backend Processes
- One backend process per client connection
- Handles all queries from that specific client
- Has access to shared memory

### 3. Background Processes
PostgreSQL runs several background processes:

| Process | Purpose |
|---------|---------|
| **Background Writer** | Writes dirty buffers to disk periodically |
| **WAL Writer** | Writes Write-Ahead Log to disk |
| **Checkpointer** | Creates checkpoints for crash recovery |
| **Autovacuum** | Cleans up dead tuples automatically |
| **Stats Collector** | Collects database statistics |

### 4. Shared Memory
- **Shared Buffers** - Cache for table and index data
- **WAL Buffers** - Buffer for write-ahead log
- **Lock Tables** - Manages concurrent access

### 5. Database Cluster
- A collection of databases managed by one server instance
- Stored in a single directory (data directory)
- Contains system catalogs, configuration files, and data files

## Memory Architecture

```
┌─────────────────────────────────────────┐
│            Shared Memory                │
│  ┌─────────────┐  ┌─────────────┐      │
│  │   Shared    │  │    WAL      │      │
│  │   Buffers   │  │   Buffers   │      │
│  └─────────────┘  └─────────────┘      │
└─────────────────────────────────────────┘

┌─────────────────┐  ┌─────────────────┐
│ Backend Process │  │ Backend Process │
│  ┌───────────┐  │  │  ┌───────────┐  │
│  │   work    │  │  │  │   work    │  │
│  │   _mem    │  │  │  │   _mem    │  │
│  └───────────┘  │  │  └───────────┘  │
└─────────────────┘  └─────────────────┘
```

## Storage Structure

```
data/                     # Data directory (PGDATA)
├── base/                 # Database files
│   ├── 1/               # template1 database
│   ├── 13067/           # Other databases (OID as folder name)
├── global/              # Cluster-wide tables
├── pg_wal/              # Write-ahead log files
├── pg_xact/             # Transaction commit status
├── postgresql.conf      # Main configuration
├── pg_hba.conf          # Client authentication
└── pg_ident.conf        # User name mapping
```

## How a Query Executes

1. **Client** sends query to server
2. **Parser** checks syntax and creates parse tree
3. **Analyzer** resolves references, checks semantics
4. **Rewriter** applies rules and views
5. **Planner/Optimizer** creates execution plan
6. **Executor** runs the plan and returns results

```
Query → Parser → Analyzer → Rewriter → Planner → Executor → Results
```

