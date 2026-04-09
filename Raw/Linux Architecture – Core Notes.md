
# Architecture
---

Linux is a Unix-like operating system built around a layered architecture that separates user‑level applications from hardware‑level operations.

## High‑Level Architecture Overview

```
Applications
Shell / CLI
System Libraries (glibc)
Kernel
Hardware
```

### Layer Description

| Layer             | Description                            |
| ----------------- | -------------------------------------- |
| Applications      | Programs executed by the user          |
| Shell / CLI       | Interface interpreting commands        |
| Libraries (glibc) | Common functions and APIs for programs |
| Kernel            | Core OS, manages resources             |
| Hardware          | CPU, RAM, Disk, Network, Devices       |

## Architecture Diagram

```
+------------------------------------+
|          User Applications         |
+------------------------------------+
|            Shell / CLI             |
+------------------------------------+
|       System Libraries (glibc)     |
+------------------------------------+
|               Kernel               |
+------------------------------------+
|              Hardware              |
+------------------------------------+
```

## Kernel Overview

The Linux Kernel is **monolithic** and manages all system operations such as:

* Process scheduling
* Memory management
* Filesystem management
* Networking stack
* I/O operations
* Device drivers
* Security and access control

### Kernel Subsystems

| Subsystem                 | Role                              |
| ------------------------- | --------------------------------- |
| Scheduler                 | Allocates CPU time to tasks       |
| Memory Manager            | Handles RAM, virtual memory, swap |
| VFS (Virtual File System) | Unified interface for filesystems |
| Device Drivers            | Communication with hardware       |
| Networking Stack          | TCP/IP handling and routing       |
| Security Module           | Access control, authentication    |

```
        +----------------------+
        |       KERNEL         |
        |----------------------|
        | Scheduler            |
        | Memory Manager       |
        | VFS                  |
        | Networking           |
        | Drivers              |
        +----------------------+
```

## Shell & System Calls

The shell translates user commands into system calls that the kernel executes.

Example:

```bash
ls
```

Execution Flow:

```
User → Shell → System Call → Kernel → Hardware/Memory → Output
```

## User Space vs Kernel Space

| Aspect       | User Space              | Kernel Space              |
| ------------ | ----------------------- | ------------------------- |
| Runs         | Applications & commands | Kernel + modules          |
| Access       | Limited                 | Full hardware access      |
| Memory       | Virtual memory          | Virtual + physical memory |
| Crash Effect | Crashes application     | Can crash the OS          |

## Linux Boot Process (Simplified)

```
Hardware → BIOS/UEFI → GRUB → Kernel → init/systemd → Services → Login
```

| Stage           | Task                                     |
| --------------- | ---------------------------------------- |
| BIOS / UEFI     | Initialize hardware                      |
| GRUB            | Loads kernel & initramfs into memory     |
| Kernel          | Detects hardware, mounts root filesystem |
| systemd (PID 1) | Starts services & user environment       |

## systemd Service Control

```bash
systemctl status
systemctl start <service>
systemctl stop <service>
systemctl restart <service>
systemctl enable <service>
```

## Linux Filesystem Structure

```
/
├── /etc     # Configuration files
├── /bin     # Basic commands
├── /sbin    # Admin commands
├── /home    # User directories
├── /var     # Logs & variable data
├── /tmp     # Temporary files
├── /dev     # Device nodes
└── /proc    # Kernel & process info
```

## Summary

```
Applications → Shell → Libraries → Kernel → Hardware
```

The kernel acts as the bridge between applications and hardware, handling core OS responsibilities such as:

* Process management
* Storage & I/O
* Memory allocation
* Networking
* Security enforcement
* Device interaction