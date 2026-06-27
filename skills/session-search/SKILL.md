---
name: session-search
version: 1.0.0
category: devops
description: Session history search — FTS5 query syntax, session browsing, window scrolling, and cross-profile session access.
tags: [session, search, history, fts5, session_search]
---

# session_search Tool

Best practices for searching Hermes conversation history using the session_search tool.

## Tool Syntax

```python
session_search(query="keyword", limit=3, sort="newest")
session_search(session_id="...", around_message_id=12345, window=5)
session_search(session_id="...", profile="work")
session_search()  # browse recent sessions
```

## Query Patterns

### Basic keyword search
```python
session_search(query="git clone", limit=5)
```

### Boolean search
```python
session_search(query="python AND docker", limit=3)
session_search(query="api NOT rest", limit=3)
```

### Browse recent sessions
```python
session_search()  # no args = chronological browse
```

## Scroll Through Session

```python
# Forward
session_search(session_id="abc123", around_message_id=last_id, window=5)
```

## Error Handling & Retry

If a query fails with timeout or connection error:
1. Wait 2 seconds, then retry up to 3 times with exponential backoff (2s → 4s → 8s)
2. If all retries fail, return a clear error message suggesting the user try a simpler query
3. Log the error: `session_search(query="...", error="timeout")` for later analysis

## Anti-patterns

- Using session_search for current data → use direct tools instead
- Not specifying limit → gets default 3, may miss relevant sessions
