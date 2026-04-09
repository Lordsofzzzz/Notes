---
status: seedling
tags: [web-security, moc, methodology, security]
created: 2026-04-10
updated: 2026-04-10
---

# Web Application Security (MOC)

## Summary
A central Map of Content (MOC) for web application security, integrating structured methodologies, testing phases, and vulnerability deep-dives.

## Details
This MOC organizes the vault's web security knowledge into a professional Red Team workflow. It synthesizes the high-level methodology with specific tactical notes.

### 1. Engagement Methodology
The structured approach to a web security assessment.
- **Phase 1: Reconnaissance**: Passive (subfinder, amass) and Active tech fingerprinting.
- **Phase 2: Scanning**: Service discovery (nmap) and automated vulnerability scanning (nuclei, nikto).
- **Phase 3: Fuzzing & Enumeration**: Deep-diving into endpoints and parameters (ffuf, gobuster).
- **Phase 4: Exploitation**: Manual and automated testing of identified flaws.
- **Phase 5: Post-Exploitation & Reporting**: Pivoting and documenting findings.

### 2. Tactical Testing Guides
- [[API Endpoint Testing]] — Discovery and security testing of API interfaces.
- [[WAF Bypass Techniques]] — Evading Web Application Firewalls.
- [[CORS (Cross-Origin Resource Sharing)]] — Testing browser-based access controls.
- [[Red Team Tools]] — Essential toolkit for assessments.

### 3. Methodology
- [[Step-by-Step Web App Red Teaming]] — Phased guide to execution.
- [[Parameters in API Enumeration]] — Discovering undocumented inputs.

### 4. Vulnerability Deep Dives
- [[What Leads to API Vulnerabilities]] — Root causes of API security flaws.
- [[How to Know if an API Has Authentication]] — Fingerprinting auth layers.
- **Injection**: SQLi (sqlmap), Command Injection.
- **Access Control**: IDOR, Broken Authentication, JWT manipulation.
- **Client-Side**: XSS (Reflected/Stored/DOM), CSRF.
- **Infrastructure**: SSRF, File Upload vulnerabilities.

### 5. Essential Tooling
- **Burp Suite**: The primary proxy and manipulation tool.
- **ffuf**: High-speed fuzzing for discovery.
- **sqlmap**: Automated SQL injection exploitation.

## Associative Trails
This MOC serves as the "brain" for web security in the vault. It bridges the gap between high-level methodology and granular tactical notes, ensuring that every individual technique is placed within the context of a professional security engagement.

## Connections
- [[Red Team Operational Security (OpSec)]]
- [[C2 Infrastructure Hosting (Red Team)]]

## Sources
- Web Application Security.md
- redteam_conversation (1).md
- redteam-complete-guide.md
