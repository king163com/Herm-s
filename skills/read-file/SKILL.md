---
name: read-file
version: 1.0.0
category: devops
description: File reading best practices — when to use read_file vs terminal cat, pagination, large file handling, encoding issues.
tags: [file, read, read_file, terminal, pagination]
---

# read_file Tool

Best practices for reading files using the read_file tool. Use this instead of terminal cat/head/tail commands.

## When to Use

- Reading source code files
- Reading configuration files
- Reading text documents (markdown, json, yaml, etc.)
- Large files with pagination (use offset + limit)
- Binary files that need special handling

## Tool Syntax

```
read_file(path="/absolute/path", offset=1, limit=500)
```

Parameters:
- `path`: Absolute path (supports ~/ shorthand)
- `offset`: Line number to start (1-indexed, default 1)
- `limit`: Max lines to read (default 500, max 2000)

## Common Patterns

### Read entire small file
```
read_file(path="~/.hermes/config.yaml")
```

### Read with pagination (large files)
```
read_file(path="/var/log/app.log", offset=1, limit=500)   # first page
read_file(path="/var/log/app.log", offset=501, limit=500) # second page
```

### Read specific section
```
read_file(path="/path/to/file", offset=100, limit=50)
```

## Anti-patterns

- Using terminal + cat to read files → use read_file instead
- Reading files >100KB without pagination
- Using head/tail in terminal → use offset/limit in read_file
