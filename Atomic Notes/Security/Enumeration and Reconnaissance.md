---
status: seedling
tags: [security, reconnaissance, enumeration]
created: 2026-04-19
updated: 2026-04-19
---

# Enumeration and Reconnaissance

## Summary
The systematic process of gathering information about a target using both passive (non-intrusive) and active (intrusive) methods to identify potential attack vectors.

## Details
**Core Logical Content Mandate**:
Reconnaissance is the foundational phase of any assessment. It is divided into passive gathering (no direct contact with target infrastructure) and active gathering (direct interaction with target services).

### Passive Information Gathering
Methods used to gather data from third-party sources or public records.
| Tool | Use |
|------|-----|
| `whois` | Retrieving domain registration data and contact information. |
| Google dorks | Using advanced operators (`site:`, `filetype:`, `inurl:`) to find sensitive files. |
| Netcraft | Analyzing the tech stack and hosting history of a web application. |
| Shodan | Searching for internet-connected devices and services. |
| GitHub recon | Searching repositories for leaked API keys, credentials, or configurations. |
| Security Headers | Analyzing SSL/TLS configurations and security-related HTTP headers. |

### Active Information Gathering
Directly probing the target's infrastructure to discover open ports and running services.
| Tool | Use |
|------|-----|
| `nmap` | Port scanning, OS fingerprinting, and service discovery via NSE scripts. |
| SMB enum | Discovering network shares, users, and testing for null sessions. |
| SMTP enum | Enumerating valid users via `VRFY` and `EXPN` commands. |
| SNMP enum | Probing community strings (e.g., `public`) to extract device configurations. |

### Vulnerability Scanning
Using automated tools to identify known security flaws.
- **Nessus**: Comprehensive scanning for vulnerabilities (credentialed vs. uncredentialed).
- **Nmap NSE**: Utilizing specific scripts for vulnerability detection (`--script vuln`).

## Associative Trails
Effective enumeration minimizes "blind" exploitation attempts and helps prioritize targets based on the actual services and vulnerabilities discovered.

## Connections
- [[Red Team Tools]]
- [[API Endpoint Testing]]

## Sources
- [[PEN-200-notes.md]]
