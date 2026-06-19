---
status: seedling
tags: [linux, sysadmin, basics]
created: 2026-04-19
updated: 2026-04-19
---

# Linux Command Line Basics

## Summary
Foundational commands for navigating the Linux filesystem and managing files via the terminal.

## Details
**Core Logical Content Mandate**:
The Linux command line is the primary interface for system administration and penetration testing.

### Navigation and Filesystem
| Command | Action |
|---------|--------|
| `pwd` | Print working directory. |
| `cd <dir>` | Change directory. |
| `ls -la` | List files in long format, including hidden files. |
| `cat file` | Print file contents. |
| `head -n 3` / `tail -n 3` | View the first or last 3 lines of a file. |

### Paths
- **Absolute**: Starting from root (e.g., `/etc/passwd`).
- **Relative**: Relative to current location (e.g., `../../etc/passwd`).
- `~` is a shortcut for the current user's home directory.

### File Management
- `touch file`: Create empty file.
- `rm -rf dir`: Recursively delete a directory and its contents.
- `cp src dst` / `mv src dst`: Copy or move/rename files.
- `ln -s target link`: Create a symbolic (soft) link.
- `find / -name file`: Search the entire filesystem for a file by name.

### Piping and Redirection
- `cmd > file`: Redirect output to file (overwrite).
- `cmd >> file`: Append output to file.
- `cmd1 | cmd2`: Pipe the output of `cmd1` into the input of `cmd2`.
- `2>&1`: Redirect standard error to standard output.

## Associative Trails
This note covers the "muscle memory" commands required for any Linux-based workflow, serving as a prerequisite for more complex administration tasks.

## Connections
- [[Linux Filesystem Hierarchy]]
- [[Linux Systems (MOC)]]

## Sources
- [[PEN-100-notes.md]]
