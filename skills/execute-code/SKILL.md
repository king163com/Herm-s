---
name: execute-code
version: 1.0.0
category: devops
description: Python script execution via execute_code tool — importing hermes_tools, error handling, loops, file I/O, and subprocess patterns.
tags: [python, execute_code, script, hermes_tools, subprocess]
---

# execute_code Tool

Best practices for running Python scripts via the execute_code tool. The tool exposes hermes_tools with terminal, read_file, patch, write_file, search_files.

## Import hermes_tools

```python
from hermes_tools import terminal, read_file, patch, write_file, search_files
```

## Common Patterns

### Read file + process
```python
from hermes_tools import read_file, terminal
result = read_file(path="/path/to/file")
# process result['content']
```

### Terminal command with error handling
```python
from hermes_tools import terminal
r = terminal(command="ls -la", timeout=10)
if r['exit_code'] != 0:
    print(f"Error: {r['output']}")
else:
    print(r['output'])
```

### Write file
```python
from hermes_tools import write_file
write_file(path="/tmp/output.txt", content="hello world")
```

### Loop with tool calls
```python
from hermes_tools import terminal, read_file
for i in range(10):
    r = terminal(command=f"echo {i}", timeout=5)
    print(r['output'])
```

## Anti-patterns

- No error handling around tool calls
- Using subprocess.run() when hermes_tools terminal() is available
- Forgetting to check exit_code
- Long-running loops without timeout
