---
status: seedling
tags: [red-team, opsec, infrastructure, security]
created: 2026-04-10
updated: 2026-04-10
---

# Red Team Operational Security (OpSec)

## Summary
The systematic process of protecting sensitive information and activities from adversaries through detection and attribution avoidance.

## Details
Process of protecting sensitive information and activities from adversaries. In red teaming, it focuses on avoiding detection and attribution.

### Core Principles
- **Separation of Identity**: Never mix real identity with red team infrastructure (dedicated VMs, separate accounts).
- **Attribution Avoidance**: Use VPN/Proxy chains and residential proxies to mask origin IPs.
- **Infrastructure Hygiene**: Never reuse infrastructure across clients; use redirectors to protect C2 servers.
- **Communication Security**: Use E2EE channels (Signal) and established communication plans.

### Infrastructure Management
- **Redirectors**: Cloud instances that forward traffic to the C2, acting as a buffer.
- **Domain Aging**: Registering domains months in advance to avoid "fresh domain" filters.
- **Malleable C2 Profiles**: Blending C2 traffic into legitimate HTTP/S traffic (e.g., mimicking Office 365).

### Tooling Hygiene
- **Custom Tooling**: Modifying public tools (Cobalt Strike, Metasploit) to avoid known signatures.
- **In-Memory Execution**: Avoiding disk-based payloads to bypass file-system monitoring.
- **Metadata Cleaning**: Stripping EXIF and author data from all engagement files.

### Legal and Scoping
- **Rules of Engagement (RoE)**: A signed document defining authorized actions, IPs, and timeframes.
- **Get-out-of-jail Letter**: A copy of the RoE for physical or digital encounters.
- **Logging**: Detailed timestamps and screenshots of every action for deconfliction.

## Associative Trails
High-end red teaming is as much about stealth as it is about exploitation. This note exists to document the defensive side of red team operations, refining the purely technical methodology with a security and attribution-avoidance mindset.

## Connections
- [[VPN and Proxy Setup (Red Team)]]
- [[C2 Infrastructure Hosting (Red Team)]]
- [[Web Application Security (MOC)]]

## Sources
- redteam_conversation (1).md
