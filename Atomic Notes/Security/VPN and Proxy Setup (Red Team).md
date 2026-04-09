---
status: seedling
tags: [red-team, networking, privacy, security]
created: 2026-04-10
updated: 2026-04-10
---

# VPN and Proxy Setup (Red Team)

## Summary
A multi-layered routing model designed to ensure anonymity and secure communication during red team operations.

## Details
Multi-layered routing model for securing and anonymizing red team traffic.

### Layered Traffic Model
```
You → VPN → Proxy/Redirector → C2 Infrastructure → Target
```

### Layers of Anonymity
- **Base Layer (VPN)**: Use a paid, no-log VPN (e.g., Mullvad, IVPN) paid with Monero.
- **Cloud Redirectors**: Expendable VPS instances (DigitalOcean, Vultr) used to forward traffic to the real C2.
- **Residential Proxies**: Legitimate ISP addresses (Brightdata, Oxylabs) for phishing or OSINT to avoid "datacenter IP" blacklists.
- **SOCKS5 via SSH**: Creating encrypted tunnels through jump hosts for tool-specific routing.

### Tools and Configuration
- **iptables**: Used on redirectors to forward ports (e.g., 443 to the C2).
- **proxychains**: A tool to force any software through a chain of proxies.
- **socat**: Alternative to iptables for port forwarding.

### Verification
- **ifconfig.me**: Check exit IPv4.
- **whoami.akamai.net**: Check for DNS leaks.
- **Leak Test**: Ensure IPv6 is disabled or routed to prevent leakage.

### Use Cases
| Scenario | Tooling |
|---|---|
| General Recon | VPN only |
| Phishing | Residential Proxies |
| Exploitation | VPN → Redirector → C2 |
| Pivoting | SSH SOCKS5 tunnel |

## Associative Trails
Anonymity is not a single tool but a multi-layered architectural approach. This note documents the operational standard for traffic routing, refining the broader [[Red Team Operational Security (OpSec)]] and challenging the reliance on single-hop VPNs.

## Connections
- [[Red Team Operational Security (OpSec)]]
- [[C2 Infrastructure Hosting (Red Team)]]

## Sources
- redteam_conversation (1).md
