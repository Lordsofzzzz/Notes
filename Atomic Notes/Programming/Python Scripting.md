---
status: seedling
tags: [programming, python, scripting]
created: 2026-04-19
updated: 2026-04-19
---

# Python Scripting

## Summary
General-purpose scripting for security automation, networking, and rapid tool development.

## Details
**Core Logical Content Mandate**:
Python is the standard for custom exploit development and complex automation due to its extensive library support.

### Basics
- **Shebang**: `#!/usr/bin/env python3`.
- **Types**: `str`, `int`, `float`, `bool`.
- **Data Structures**:
    - **Lists**: `lst = ["a", "b"]`, `lst.append("c")`.
    - **Dicts**: `d = {"key": "value"}`, accessed via `d["key"]`.

### Control Flow
```python
if x > 5:
    pass
for item in ["a", "b"]:
    print(item)
while condition:
    pass
```

### Network and HTTP
Python excels at interacting with web services and raw sockets.
- **Requests Library**:
    ```python
    import requests
    r = requests.get("http://target")
    print(r.status_code, r.text)
    ```
- **Sockets**:
    ```python
    import socket
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect(("host", 80))
    s.send(b"GET / HTTP/1.0\r\n\r\n")
    ```

### File I/O
```python
with open("file.txt", "r") as f:
    content = f.read()
with open("file.txt", "w") as f:
    f.write("data")
```

## Associative Trails
Python bridges the gap between simple shell automation and full-scale tool development, especially for network-based attacks.

## Connections
- [[Bash Scripting]]
- [[PowerShell Scripting]]

## Sources
- [[PEN-100-notes.md]]
