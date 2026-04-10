---
status: seedling
tags: [linux, architecture, system-design]
created: 2026-04-10
updated: 2026-04-10
---

# Linux Architecture (Core)

## Summary
A layered system architecture that provides isolation between user applications and hardware through the kernel and system libraries.

## Details
Layered system that separates user-level applications from hardware operations.

### Layered Structure
1. **Applications**: User programs (e.g., browsers, editors).
2. **Shell / CLI**: Interprets user commands into system calls.
3. **System Libraries (glibc)**: Provides standard APIs for programs to interact with the kernel.
4. **Kernel**: The core of the OS, managing system resources.
5. **Hardware**: CPU, RAM, Disk, Network devices.

### The Linux Kernel
The kernel is **monolithic** and responsible for the following subsystems:

| Subsystem | Role |
| :--- | :--- |
| **Scheduler** | Allocates CPU time to tasks. |
| **Memory Manager** | Handles RAM, virtual memory, and swap. |
| **VFS (Virtual File System)** | Provides a unified interface for different filesystems. |
| **Device Drivers** | Communicates with hardware components. |
| **Networking Stack** | Manages TCP/IP handling and routing. |
| **Security Module** | Handles access control and authentication. |

### Execution Flow
```
User → Shell → System Call → Kernel → Hardware/Memory → Output
```

### User Space vs. Kernel Space
| Aspect | User Space | Kernel Space |
| :--- | :--- | :--- |
| **Runs** | Applications & commands | Kernel + modules |
| **Access** | Limited hardware access | Full hardware access |
| **Memory** | Virtual memory only | Virtual + physical memory |
| **Crash Effect** | Crashes the specific application | Can crash the entire OS |

### Linux Boot Process (Simplified)
```
Hardware → BIOS/UEFI → GRUB → Kernel → init/systemd → Services → Login
```

| Stage | Task |
| :--- | :--- |
| **BIOS / UEFI** | Initialize hardware components. |
| **GRUB** | Loads kernel and initramfs into memory. |
| **Kernel** | Detects hardware and mounts root filesystem. |
| **systemd (PID 1)** | Starts system services and user environment. |

## Associative Trails
Understanding the core layers is crucial for system administration and high-performance development. This note exists to document the fundamental separation of concerns in the Linux OS, refining the understanding of how user commands eventually interact with physical hardware.

## Connections
- [[Linux Systems (MOC)]]
- [[Linux GUI and OS Layers]]
- [[Linux Boot Process]]
- [[Linux Filesystem Hierarchy]]

## Sources
- Linux Architecture – Core Notes.md
