---
status: seedling
tags: [windows, cmd, basics]
created: 2026-04-19
updated: 2026-04-19
---

# Windows Command Line Essentials

## Summary
Core CMD commands for navigating the Windows filesystem, managing files, and gathering system information.

## Details
**Core Logical Content Mandate**:
CMD is the legacy but still ubiquitous command-line interface for Windows systems.

### Navigation and System Info
| Command | Action |
|---------|--------|
| `dir /A` | List directory contents including hidden files. |
| `dir /s *.exe` | Search recursively for `.exe` files. |
| `type file.txt` | Print file contents. |
| `systeminfo` | Display full system configuration. |
| `whoami /user` | Show current user and their SID. |

### Environment Variables
Access using `%VAR%` notation.
- `%PATH%`: Executable search path.
- `%USERPROFILE%`: Current user's home directory.
- `%TEMP%`: Temporary file storage.

### File Management
- `echo text > file.txt`: Create or overwrite file.
- `del file.txt` / `rmdir /S dir`: Delete files or directories (with contents).
- `ren old new` / `move file dest`: Rename or move files.
- `icacls file`: View or modify Access Control Lists (permissions).
    - `F`=Full, `M`=Modify, `RX`=Read+Execute.

### Registry and Tasks
- `reg query <key>`: View registry entries (e.g., `HKCU\...\Run` for persistence).
- `schtasks`: List or create scheduled tasks.

## Associative Trails
This note provides the Windows counterpart to Linux CLI basics, essential for manual enumeration when a shell is gained on a Windows target.

## Connections
- [[Windows Privilege Escalation]]
- [[Active Directory Basics]]

## Sources
- [[PEN-100-notes.md]]
