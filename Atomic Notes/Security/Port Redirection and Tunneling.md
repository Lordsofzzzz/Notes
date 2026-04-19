---
status: seedling
tags: [security, networking, pivoting, tunneling]
created: 2026-04-19
updated: 2026-04-19
---

# Port Redirection and Tunneling

## Summary
The practice of routing traffic through a compromised host to access services on isolated internal networks that are not directly reachable from the attacker's machine.

## Details
**Core Logical Content Mandate**:
Pivoting allows an attacker to bypass firewalls and reach internal infrastructure by using a compromised machine as a "jump host."

### Core Techniques
- **SSH Tunneling**:
    - **Local Forwarding (`-L`)**: Accessing a remote service on the jump host's network.
    - **Remote Forwarding (`-R`)**: Exposing a service on the attacker's machine to the remote network.
    - **Dynamic Forwarding (`-D`)**: Creating a SOCKS proxy for routing traffic through the jump host.
- **Chisel**: A fast TCP/UDP tunnel over HTTP, useful for bypassing restricted egress filtering. Supports SOCKS5.
- **Ligolo-ng**: A modern, high-performance tunneling tool that uses a TUN interface for easier network routing without requiring SOCKS proxies for every command.
- **Metasploit Pivoting**: Using the `route add` command and `SOCKS proxy` post-exploitation modules.

### Logical Application
1. **Compromise Host A** (Dual-homed: External and Internal network).
2. **Setup Tunnel** on Host A (e.g., Chisel or SSH).
3. **Route Traffic** from the attacker machine through Host A to reach **Internal Host B**.

## Associative Trails
Pivoting is the technical "bridge" that enables the transition from external reconnaissance to internal network exploitation.

## Connections
- [[Active Directory Lateral Movement]]
- [[Linux Privilege Escalation]]

## Sources
- [[PEN-200-notes.md]]
