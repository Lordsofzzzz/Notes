---
status: seedling
tags: [windows, active-directory, basics]
created: 2026-04-19
updated: 2026-04-19
---

# Active Directory Basics

## Summary
Foundational concepts of Microsoft's centralized identity and access management system for Windows networks.

## Details
**Core Logical Content Mandate**:
Active Directory (AD) is the backbone of enterprise networks, managing users, computers, and permissions.

### Core Components
- **Domain Controller (DC)**: The server that manages the AD database (NTDS.dit) and handles authentication requests.
- **Objects**:
    - **Users**: Individual accounts.
    - **Groups**: Collections of users for permission management.
    - **Computers**: Workstations and servers joined to the domain.
    - **OUs (Organizational Units)**: Containers used to organize objects and apply Group Policies.
- **Group Policy Objects (GPOs)**: Sets of rules and configurations applied to users or computers.

### Logical Structure
- **Domain**: A logical group of network objects.
- **Forest**: The highest-level container, consisting of one or more domains that share a common schema and trust.
- **Trust**: A relationship between domains that allows users in one domain to access resources in another.

### Enumeration (PowerShell)
Using the Active Directory module (requires RSAT or an existing session on a DC):
- `Get-ADUser -Identity <name>`: View user details.
- `Get-ADGroupMember "Domain Admins"`: List members of a sensitive group.
- `Get-ADDomain`: View current domain information.

## Associative Trails
A solid grasp of AD structure is required before attempting more advanced attacks like Kerberoasting or DCSync.

## Connections
- [[Active Directory Authentication Attacks]]
- [[Active Directory Lateral Movement]]

## Sources
- [[PEN-100-notes.md]]
