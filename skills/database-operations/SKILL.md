---
name: database-operations
version: 1.0.0
category: devops
description: Database operations patterns — SQLite connections, SQL queries, data export, schema inspection, and backup.
tags: [database, sqlite, sql, backup, export]
---

# Database Operations

Patterns for working with SQLite databases (Hermes uses SQLite for sessions, skills, etc.)

## Connect to SQLite

```python
import sqlite3
db = sqlite3.connect("/path/to/database.db")
cursor = db.cursor()
```

## Common Operations

### List tables
```python
cursor.execute("SELECT name FROM sqlite_master WHERE type='table'")
tables = cursor.fetchall()
```

### Query data
```python
cursor.execute("SELECT * FROM table_name LIMIT 10")
rows = cursor.fetchall()
```

### Export to CSV
```python
import csv
with open("output.csv", "w") as f:
    writer = csv.writer(f)
    writer.writerows(data)
```

## Anti-patterns

- Using string formatting for SQL → use parameterized queries
- Not closing connections
