---
status: seedling
tags: [security, active-directory, kerberos, authentication]
created: 2026-04-19
updated: 2026-04-19
---

# Active Directory Authentication Attacks

## Summary
Exploiting weaknesses in Active Directory authentication protocols (Kerberos, NTLM) to gain unauthorized access or elevate privileges.

## Details
**Core Logical Content Mandate**:
Active Directory authentication relies heavily on Kerberos and NTLM. Attacks often target the lack of pre-authentication, weak service account passwords, or the ability to forge tickets.

### Core Authentication Attacks
| Attack | Tool | Technique |
|--------|------|-----------|
| **Password Spraying** | `kerbrute`, `CrackMapExec` | Testing one common password against many users to avoid account lockout policies. |
| **AS-REP Roasting** | `GetNPUsers.py` | Targeting users with "Do not require Kerberos pre-authentication" set. The DC returns an AS-REP which can be cracked offline. |
| **Kerberoasting** | `GetUserSPNs.py` | Requesting a Service Ticket (TGS) for any account with a Service Principal Name (SPN). The TGS is encrypted with the service account's hash and can be cracked offline. |
| **Silver Ticket** | `mimikatz` | Forging a Ticket Granting Service (TGS) ticket for a specific service using a compromised service account hash. |
| **DCSync** | `mimikatz` | Utilizing `DS-Replication-Get-Changes` rights to simulate a Domain Controller and pull hashes from the NTDS.dit. |

### Technical Execution Logic
- **AS-REP Roasting**: Focus on accounts that don't require pre-authentication.
- **Kerberoasting**: 
    1. Identify SPNs in the domain.
    2. Request TGS for the target SPN.
    3. Export the ticket and crack it using `hashcat` or `john`.

## Associative Trails
Authentication attacks are often the first step in moving from a standard domain user to a privileged account or Domain Admin within an AD environment.

## Connections
- [[Active Directory Lateral Movement]]
- [[Password Attacks]]

## Sources
- [[PEN-200-notes.md]]
