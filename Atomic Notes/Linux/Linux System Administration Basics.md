---
status: seedling
tags: [linux, permissions, processes, sysadmin]
created: 2026-04-19
updated: 2026-04-19
---

# Linux System Administration Basics

## Summary
Core concepts of Linux administration, including user management, file permissions, process control, and scheduled tasks.

## Details
**Core Logical Content Mandate**:
System administration involves managing the multi-user environment and ensuring services run correctly.

### Users and Permissions
- **Files**: `/etc/passwd` (accounts), `/etc/shadow` (hashes), `/etc/group` (memberships).
- **Permissions**: Represented by `rwx` for Owner, Group, and Others.
    - `r=4, w=2, x=1`.
    - `chmod 755`: `rwxr-xr-x`.
- **Special Bits**:
    - **SUID (4)**: Binary runs with the permissions of the owner (e.g., `passwd`).
    - **SGID (2)**: Binary runs with the permissions of the group.

### Process Management
- `ps aux`: List all running processes.
- `kill -9 PID`: Forcefully terminate a process.
- `jobs` / `fg` / `bg`: Manage background and foreground jobs.

### Package and Service Management
- **APT (Debian/Kali)**: `apt update`, `apt install pkg`, `apt remove pkg`.
- **systemd**:
    - `systemctl start <service>`: Start a service.
    - `systemctl enable <service>`: Set service to start on boot.

### Scheduled Tasks (cron)
Managed via `crontab -l` or `/etc/crontab`.
- Format: `min hr day month weekday command`.

## Associative Trails
Understanding these basics is critical for privilege escalation, as misconfigurations in permissions or cron jobs are primary attack vectors.

## Connections
- [[Linux Privilege Escalation]]
- [[systemd (Service Control)]]

## Sources
- [[PEN-100-notes.md]]
