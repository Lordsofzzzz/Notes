---
status: seedling
tags: [red-team, infrastructure, c2, security]
created: 2026-04-10
updated: 2026-04-10
---

# C2 Infrastructure Hosting (Red Team)

## Summary
The strategic architectural layout for hosting Command and Control (C2) servers to maximize stealth and longevity.

## Details
Architecture and security of Command and Control (C2) servers.

### Golden Rule of C2 Hosting
**Redirectors are public-facing. C2 is never directly exposed.**

### Infrastructure Architecture
```
Internet
    │
    ▼
Redirectors (Cloud/CDN VPS) ← Public-facing, expendable
    │
    ▼
C2 Server (VPS/Dedicated)   ← Hidden, never public
    │
    ▼
Team Server Access          ← Accessible only via VPN
```

### Hosting Providers
- **Vultr**: Crypto payments, global regions.
- **DigitalOcean**: Fast API for automation.
- **Hetzner**: Cheap, European jurisdiction (Finland, Germany).
- **Linode**: Less scrutiny than AWS/Azure.

### Server Hardening
- **Firewall (UFW)**: Deny all incoming except from redirector IPs and admin VPN.
- **SSH Hardening**: Use keys only, disable passwords, run as non-root user.
- **C2 Configuration**: Change default ports (e.g., Cobalt Strike 50050) to avoid automatic scanning.

### Payment and Identity
- **Anonymity**: Pay with **Monero (XMR)** to break financial tracking.
- **Pseudonyms**: Use burner email addresses (ProtonMail) over VPN.

### Jurisdiction
- **Low-Risk Jurisdictions**: Iceland, Switzerland, Romania, Netherlands.

## Associative Trails
Infrastructure is the most vulnerable part of an operation to discovery; stealth is paramount. This note centralizes architecture patterns that protect the C2 server by using expendable redirectors, challenging the common mistake of direct exposure.

## Connections
- [[VPN and Proxy Setup (Red Team)]]
- [[Red Team Operational Security (OpSec)]]

## Sources
- [[redteam_conversation (1).md]]
