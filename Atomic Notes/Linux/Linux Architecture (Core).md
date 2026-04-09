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
The kernel is **monolithic** and responsible for:
- **Process Scheduling**: Allocating CPU time.
- **Memory Management**: Handling RAM and virtual memory.
- **VFS (Virtual File System)**: Providing a unified interface for different filesystems.
- **Device Drivers**: Communicating with hardware components.
- **Networking Stack**: Managing TCP/IP and routing.
- **Security**: Access control and authentication.

### Execution Flow
```
User → Shell → System Call → Kernel → Hardware/Memory → Output
```

### User Space vs. Kernel Space
- **User Space**: Where applications run; limited hardware access; crashes only affect the app.
- **Kernel Space**: Where the kernel and modules run; full hardware access; crashes can take down the entire system.

## Associative Trails
Understanding the core layers is crucial for system administration and high-performance development. This note exists to document the fundamental separation of concerns in the Linux OS, refining the understanding of how user commands eventually interact with physical hardware.

## Connections
- [[Linux Systems (MOC)]]
- [[Linux GUI and OS Layers]]
- [[Linux Boot Process]]
- [[Linux Filesystem Hierarchy]]

## Sources
- Linux Architecture – Core Notes.md
