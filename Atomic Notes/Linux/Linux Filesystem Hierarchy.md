---
status: seedling
tags: [linux, filesystem, fhs]
created: 2026-04-10
updated: 2026-04-10
---

# Linux Filesystem Hierarchy

## Summary
The standardized directory structure on a Linux system, following the Filesystem Hierarchy Standard (FHS).

## Details
Standard directory structure on a Linux system.

### Common Directories
- `/`: The root directory.
- `/etc`: Configuration files for system-wide settings.
- `/bin`: Basic user commands (binaries).
- `/sbin`: Administrative commands (system binaries).
- `/home`: User personal directories (e.g., `/home/username`).
- `/var`: Variable data (logs, spool files, databases).
- `/tmp`: Temporary files (often cleared on reboot).
- `/dev`: Device nodes for hardware (e.g., `/dev/sda1`).
- `/proc`: Virtual filesystem for kernel and process information.
- `/root`: Home directory for the root user.

## Associative Trails
A standardized map for navigating and managing any Linux distribution. This note documents the Filesystem Hierarchy Standard (FHS), refining the transition for users coming from non-standard directory layouts and challenging the "files everywhere" approach.

## Connections
- [[Linux Architecture (Core)]]
- [[Linux Systems (MOC)]]
- [[Linux Boot Process]]

## Sources
- [[Linux Architecture – Core Notes.md]]
