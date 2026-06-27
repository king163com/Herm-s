---
name: terminal-best-practices
version: 1.0.0
category: devops
description: Terminal command execution best practices — error handling, PATH issues, timeout management, and common failure patterns.
tags: [terminal, shell, bash, error-handling]
---

# Terminal Best Practices

Robust terminal command execution with proper error handling for Hermes Agent on macOS.

## When to Activate

- Using `terminal()` tool to run shell commands
- Troubleshooting "command not found" errors
- Handling long-running commands
- macOS-specific PATH issues

## Workflow

### Before Running

1. Check if command exists: `which <cmd>` or `command -v <cmd>`
2. For Python scripts, verify shebang and use `python3` explicitly (not `python`)
3. On macOS, prefer absolute paths for common tools: `/usr/bin/`, `/usr/local/bin/`

### Common Error Patterns & Fixes

| Error | Fix |
|-------|-----|
| `command not found` | Use full path or `which` to find it first |
| `Permission denied` | Check `ls -la` and use `chmod` if needed |
| `No such file or directory` | Use `realpath` or `readlink -f` to resolve symlinks |
| Timeout | Set `timeout=N` parameter (seconds); if it times out, retry up to 3 times with 2x backoff (5s → 10s → 20s) |
| Path with spaces | Always quote: `"$var"` or `path/to/'My Documents'` |
| Connection/host error | Retry with exponential backoff (2s → 4s → 8s); check network connectivity |

### Long-Running Commands

- Always set `timeout=300` for builds, `pip install`, git clones
- Use `background=true` for servers/watchers with `notify_on_complete=True`
- For loops with delays, prefer `watch_patterns` over blind `sleep`

### macOS-Specific Notes

- Homebrew PATH: `/opt/homebrew/bin:/usr/local/bin`
- `open` command for GUI apps
- `pbcopy`/`pbpaste` for clipboard
- Cron jobs need full PATH in scripts

## Anti-patterns

- ❌ `python` (use `python3`)
- ❌ Blind `sleep N` loops (use `watch_patterns` or poll with timeout)
- ❌ `kill -9` without trying graceful `kill` first
- ❌ Commands with unquoted variables containing spaces
