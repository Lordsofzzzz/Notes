---
status: seedling
tags: [networking, protocols, osi]
created: 2026-04-19
updated: 2026-04-19
---

# Networking Fundamentals

## Summary
The core models and protocols that enable communication across computer networks.

## Details
**Core Logical Content Mandate**:
Networking is governed by layered models that separate physical transmission from application-level data.

### OSI Model (7 Layers)
| Layer | Name | Examples |
|-------|------|---------|
| 7 | Application | HTTP, DNS, FTP |
| 4 | Transport | TCP, UDP |
| 3 | Network | IP, ICMP |
| 2 | Data Link | Ethernet, ARP |

### Key Protocols and Ports
- **TCP (Layer 4)**: Reliable, connection-oriented (3-way handshake).
- **UDP (Layer 4)**: Fast, connectionless.
- **ARP (Layer 2)**: Resolves IP addresses to MAC addresses.
- **DNS (Port 53)**: Resolves hostnames to IP addresses.
- **SMB (Port 445)**: Windows file sharing.
- **SSH (Port 22)**: Secure remote shell.

### IP Addressing
- **IPv4**: 32-bit addresses (e.g., `192.168.1.1`).
- **Subnet Mask**: Determines the network vs. host portion of an address.
- **CIDR**: Notation like `/24` (256 addresses) or `/16` (65,536 addresses).

### Connectivity Tools
- `ping`: Tests reachability using ICMP.
- `traceroute` / `tracert`: Identifies the path taken to a destination.
- `netstat` / `ss`: Shows active network connections and listening ports.

## Associative Trails
A deep understanding of networking is required for nearly every phase of a pentest, from scanning and enumeration to exploitation and pivoting.

## Connections
- [[Port Redirection and Tunneling]]
- [[Enumeration and Reconnaissance]]

## Sources
- [[PEN-100-notes.md]]
