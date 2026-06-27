---
name: process
version: 1.0.0
category: devops
description: Background process management — polling, log tailing, stdin writing, process termination, and output capture.
tags: [process, background, poll, kill, stdin, stdout]
---

# process Tool

Best practices for managing background processes started with terminal(background=true).

## Tool Syntax

```python
process(action="list")
process(action="poll", session_id="...")
process(action="log", session_id="...", offset=0, limit=100)
process(action="wait", session_id="...", timeout=30)
process(action="kill", session_id="...")
process(action="write", session_id="...", data="input")
```

## Common Patterns

### List all background processes
```python
process(action="list")
```

### Check status + new output
```python
process(action="poll", session_id="abc123")
```

### Wait for completion
```python
process(action="wait", session_id="abc123", timeout=60)
```

### Kill stuck process
```python
process(action="kill", session_id="abc123")
```

## Anti-patterns

- Not calling process(action="list") before trying to poll/kill
- Setting timeout too low
- Not checking exit_code after wait
