---
status: seedling
tags: [linux, boot, startup]
created: 2026-04-10
updated: 2026-04-10
---

# Linux Boot Process

## Summary
The sequence of stages a Linux system undergoes from power-on to the user login prompt.

## Details
Sequence from power-on to user login.

### Stages of the Boot Process
1. **Hardware / Firmware (BIOS/UEFI)**: Initializes hardware components (POST).
2. **Bootloader (GRUB)**: Loads the kernel and `initramfs` (initial RAM filesystem) into memory.
3. **Kernel**: Detects hardware and mounts the root filesystem (`/`).
4. **Init System (systemd)**: The first process (PID 1) that starts system services and the user environment.

### Execution Flow
```
Hardware → BIOS/UEFI → GRUB → Kernel → init/systemd → Services → Login
```

## Associative Trails
Crucial for troubleshooting non-booting systems and understanding system startup. This note documents the critical handoff points between hardware, firmware, and the OS kernel, refining the broader [[Linux Systems (MOC)]].

## Connections
- [[Linux Architecture (Core)]]
- [[Linux Systems (MOC)]]
- [[systemd (Service Control)]]

## Sources
- Linux Architecture – Core Notes.md
