---
status: seedling
tags: [security, linux, privilege-escalation]
created: 2026-04-19
updated: 2026-04-19
---

# Linux Privilege Escalation

## Summary
Techniques for escalating privileges from a standard user to root on a Linux system by exploiting misconfigurations and binary permissions.

## Details
**Core Logical Content Mandate**:
Linux privilege escalation often centers on exploiting administrative shortcuts or poorly secured system files.

### Enumeration
- **Automated Tools**: `linPEAS` provides a comprehensive overview of potential vulnerabilities.
- **Manual Vectors**:
    - **SUID/SGID**: Finding binaries that run with owner permissions: `find / -perm -4000 2>/dev/null`.
    - **Capabilities**: Checking for extended file attributes: `getcap -r / 2>/dev/null`.
    - **Cron Jobs**: Checking scheduled tasks in `/etc/crontab` and `/etc/cron.*`.
    - **Writable `/etc/passwd`**: Checking if the password file can be modified to add a root user.

### Escalation Techniques
| Vector | Method |
|--------|--------|
| **SUID Abuse** | Using legitimate binaries with the SUID bit to execute commands (refer to GTFObins). |
| **Sudo Misconfig** | Checking `sudo -l` for commands that can be run as root without a password. |
| **Cron Jobs** | Injecting a reverse shell into a script that is executed by a root-owned cron job. |
| **NFS no_root_squash**| Mounting an NFS share and creating a SUID binary to gain root access on the server. |
| **Capabilities** | Exploiting `cap_setuid+eip` to change the UID to 0 (root). |

## Associative Trails
Root access on Linux allows full control over the system, enabling the attacker to clear logs, install persistent backdoors, or pivot into internal networks.

## Connections
- [[Linux Systems (MOC)]]
- [[Port Redirection and Tunneling]]

## Sources
- [[PEN-200-notes.md]]
