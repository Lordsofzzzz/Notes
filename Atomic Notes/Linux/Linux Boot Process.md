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
| Stage | Description |
| :--- | :--- |
| **BIOS / UEFI** | Hardware / Firmware: Initializes hardware components (POST). |
| **GRUB** | Bootloader: Loads the kernel and `initramfs` (initial RAM filesystem) into memory. |
| **Kernel** | Core OS: Detects hardware, mounts the root filesystem (`/`), and initializes devices. |
| **systemd (PID 1)** | Init System: The first process that starts system services and the user environment. |

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
