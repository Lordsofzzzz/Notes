---
status: seedling
tags: [security, windows, privilege-escalation]
created: 2026-04-19
updated: 2026-04-19
---

# Windows Privilege Escalation

## Summary
The process of gaining higher-level permissions (e.g., Administrator or SYSTEM) on a compromised Windows machine.

## Details
**Core Logical Content Mandate**:
Privilege escalation involves identifying and exploiting misconfigurations, weak permissions, or kernel vulnerabilities.

### Enumeration
Before attempting an exploit, thorough enumeration of the system environment is required.
- **Automated Tools**: `winPEAS` is the standard for identifying potential escalation paths.
- **Manual Discovery**:
    - `sysinfo`: Check OS version and patches.
    - `whoami /priv`: Review current user privileges.
    - `net user`: List local users and groups.
    - **PowerShell**: Examine command history, environment variables, and script contents.

### Common Escalation Vectors
| Vector | Method |
|--------|--------|
| **Service Binary Hijack** | Replacing a service executable with a malicious binary (requires write permissions). |
| **DLL Hijacking** | Placing a malicious DLL in a directory searched by an application before the legitimate DLL location. |
| **Unquoted Service Path** | Exploiting paths with spaces and no quotes (e.g., `C:\Program Files\App Dir\bin.exe` -> `C:\Program.exe`). |
| **Scheduled Tasks** | Modifying tasks or scripts that run with elevated privileges. |
| **Token Impersonation** | Exploiting `SeImpersonatePrivilege` to assume the identity of a system service. |
| **Kernel Exploits** | Exploiting flaws in the Windows kernel (dependent on patch levels). |

## Associative Trails
Gaining SYSTEM access on a Windows host is often necessary to access sensitive data, dump credentials (LSASS), or perform lateral movement across a domain.

## Connections
- [[Antivirus Evasion]]
- [[Active Directory Lateral Movement]]

## Sources
- [[PEN-200-notes.md]]
