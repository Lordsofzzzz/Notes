---
status: seedling
tags: [linux, service-control, systemd]
created: 2026-04-10
updated: 2026-04-10
---

# systemd (Service Control)

## Summary
The modern system and service manager for Linux, acting as the first process (PID 1) to start.

## Details
Standard system and service manager for modern Linux distributions. It is the first process (PID 1) started by the kernel.

### Core Commands
- **`systemctl status <service>`**: Check the status of a service.
- **`systemctl start <service>`**: Start a service immediately.
- **`systemctl stop <service>`**: Stop a service.
- **`systemctl restart <service>`**: Restart a service.
- **`systemctl enable <service>`**: Enable a service to start automatically on boot.
- **`systemctl disable <service>`**: Prevent a service from starting on boot.

### Features
- **Parallel Start**: Starts services in parallel to reduce boot time.
- **Unit Files**: Uses `.service` files in `/etc/systemd/system/` or `/usr/lib/systemd/system/` for configuration.

## Associative Trails
The most important process in a modern Linux system, managing the entire service lifecycle. This note documents the modern SysVinit replacement, refining the understanding of PID 1 roles and providing a standard for service automation.

## Connections
- [[Linux Boot Process]]
- [[Linux Architecture (Core)]]
- [[Linux Systems (MOC)]]

## Sources
- [[Linux Architecture – Core Notes.md]]
