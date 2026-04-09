---
status: seedling
tags: [red-team, web-security, methodology, security]
created: 2026-04-10
updated: 2026-04-10
---

# Red Team Methodology (Web Application)

## Summary
The structured approach to performing a security assessment on web applications, from initial reconnaissance to final reporting.

## Details
Structured phases of a web application engagement.

### Phases of Engagement
1. **Reconnaissance**: Passive and active information gathering.
   - **Passive Recon**: `subfinder`, `amass`, Google dorking, `waybackurls`.
   - **Tech Fingerprinting**: `whatweb`, `curl -I`.
2. **Scanning**: Vulnerability and service discovery.
   - **Service Scanning**: `nmap -sC -sV`.
   - **Vulnerability Scanning**: `nikto`, `nuclei`.
   - **Fuzzing**: `ffuf`, `gobuster`.
3. **Enumeration**: Deep-diving into identified endpoints and parameters.
4. **Exploitation**: Leveraging vulnerabilities to gain unauthorized access.
5. **Post-Exploitation**: Pivoting, data exfiltration, and persistence.
6. **Reporting**: Documenting findings with remediation steps.

### Core Vulnerability Testing
- **SQL Injection (SQLi)**: `sqlmap`, manual testing (`' OR '1'='1`).
- **Cross-Site Scripting (XSS)**: Reflected, Stored, DOM-based (`<script>alert(1)</script>`).
- **Broken Authentication**: Brute force (`hydra`), session management flaws.
- **IDOR (Insecure Direct Object Reference)**: Testing with two accounts for access control.
- **File Upload**: PHP webshells, bypassing extension filters.
- **SSRF (Server-Side Request Forgery)**: Targeting metadata services or internal hosts.
- **Broken Access Control**: Privilege escalation via role changes in JWTs.

### Essential Tools
- **Burp Suite**: Proxy, Repeater, Intruder, Sitemap.
- **jwt_tool**: JWT manipulation.
- **sqlmap**: Automated SQLi.
- **ffuf**: Fast fuzzing.

## Associative Trails
Methodology turns random exploitation into a professional, reproducible security process. This note exists to define the standard phases of engagement, refining the bug hunting approach into a disciplined Red Team workflow and challenging the "scanner-only" mindset.

## Connections
- [[Red Team Operational Security (OpSec)]]
- [[API Endpoint Testing]]
- [[CORS (Cross-Origin Resource Sharing)]]
- [[WAF Bypass Techniques]]

## Sources
- [[redteam_conversation (1).md]]
