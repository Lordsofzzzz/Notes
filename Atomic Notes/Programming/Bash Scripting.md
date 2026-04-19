---
status: seedling
tags: [programming, bash, scripting, linux]
created: 2026-04-19
updated: 2026-04-19
---

# Bash Scripting

## Summary
Automation of Linux tasks using the Bourne-Again Shell (Bash), focusing on logic, loops, and command integration.

## Details
**Core Logical Content Mandate**:
Bash scripts allow for the sequencing of Linux commands with added logic and control flow.

### Essentials
- **Shebang**: `#!/bin/bash` (must be the first line).
- **Execution**: `chmod +x script.sh` followed by `./script.sh`.

### Variables and Arguments
- **Variables**: `name="Alice"`, accessed via `$name`.
- **Arguments**:
    - `$0`: Script name.
    - `$1`, `$2`: Positional arguments.
    - `$@`: All arguments.
    - `$#`: Argument count.
    - `$?`: Exit code of the last command (0 = success).

### Conditionals and Loops
**If Statement**:
```bash
if [ $num -gt 5 ]; then
    echo "big"
else
    echo "small"
fi
```
- **Operators**: `-eq` (equal), `-ne` (not equal), `-gt` (greater than), `-lt` (less than).

**Loops**:
```bash
for i in {1..10}; do
    echo $i
done

while [ $count -lt 10 ]; do
    ((count++))
done
```

### Functions
```bash
greet() {
    echo "Hello $1"
}
greet "User"
```

## Associative Trails
Bash scripting is indispensable for automating reconnaissance, log parsing, and post-exploitation tasks on Linux systems.

## Connections
- [[Linux Command Line Basics]]
- [[Python Scripting]]

## Sources
- [[PEN-100-notes.md]]
