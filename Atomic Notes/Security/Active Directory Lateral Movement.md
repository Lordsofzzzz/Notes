---
status: seedling
tags: [security, active-directory, lateral-movement]
created: 2026-04-19
updated: 2026-04-19
---

# Active Directory Lateral Movement

## Summary
Techniques for moving between systems within an Active Directory environment after an initial foothold has been established.

## Details
**Core Logical Content Mandate**:
Lateral movement leverages compromised credentials or session tokens to access other machines on the network.

### Lateral Movement Techniques
| Technique | Tool | Logic |
|-----------|------|-------|
| **WMI** | `wmic`, `impacket` | Windows Management Instrumentation for remote command execution. |
| **WinRM** | `evil-winrm` | PowerShell Remoting over HTTP/HTTPS (Port 5985/5986). |
| **PsExec** | `impacket PsExec` | Creating a remote service for shell access (requires admin rights). |
| **Pass-the-Hash (PtH)**| `CrackMapExec`, `pth-winexe` | Using an NTLM hash to authenticate instead of a plaintext password. |
| **Overpass-the-Hash** | `mimikatz` | Using an NTLM hash to request a Kerberos TGT. |
| **Pass-the-Ticket (PtT)**| `mimikatz` (inject `.kirbi`) | Injecting a stolen Kerberos ticket into the current session. |
| **DCOM** | `MMC20.Application` | Distributed Component Object Model for remote execution. |

### Pivoting & Persistence
- **Golden Ticket**: Forging a Ticket Granting Ticket (TGT) using the `KRBTGT` hash. Provides persistent access as any user for up to 10 years.
- **Shadow Copies**: Using `vssadmin` to create a shadow copy of the volume and extract `NTDS.dit` for offline analysis.

## Associative Trails
Lateral movement is the "expansion" phase of an AD assessment, allowing the attacker to reach high-value targets like Domain Controllers or file servers.

## Connections
- [[Active Directory Authentication Attacks]]
- [[Port Redirection and Tunneling]]

## Sources
- [[PEN-200-notes.md]]
