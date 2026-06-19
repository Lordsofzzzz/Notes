---
status: seedling
tags: [security, web-attacks, xss, sqli, lfi]
created: 2026-04-19
updated: 2026-04-19
---

# Web Application Attacks

## Summary
Common vulnerabilities and exploitation techniques targeting web applications, following a structured methodology from fingerprinting to post-exploitation.

## Details
**Core Logical Content Mandate**:
The methodology for web attacks follows a 4-step process: **Fingerprint -> Enumerate -> Exploit -> Post-exploit**.

### Core Vulnerabilities
- **Cross-Site Scripting (XSS)**: 
    - Types: Stored (persistent) vs. Reflected (non-persistent).
    - Payloads: JavaScript execution for cookie theft or privilege escalation via administrative actions.
- **Directory Traversal**: 
    - Mechanism: Using `../` to access files outside the web root.
    - Encoding: Bypassing filters using URL encoding (e.g., `%2e%2e%2f`).
- **LFI / RFI (File Inclusion)**: 
    - Local (LFI): Including local system files (e.g., `/etc/passwd`).
    - Remote (RFI): Including files from a remote server (requires `allow_url_include`).
    - Wrappers: Using PHP wrappers like `php://filter` to read source code.
- **File Upload**: 
    - Exploitation: Uploading executable scripts (e.g., `.php`, `.asp`).
    - Bypass: Bypassing extension filters or content-type checks.
- **Command Injection**: 
    - OS Command execution via vulnerable inputs.
    - Chaining: Using operators like `;`, `&&`, `|` to execute multiple commands.
- **SQL Injection (SQLi)**: 
    - Types: Error-based, UNION-based, Blind (Boolean or Time-based).
    - Automation: Use of `sqlmap` for detection and extraction.

### Essential Tools
| Tool | Use |
|------|-----|
| `gobuster` | Directory and file brute-forcing. |
| Burp Suite | Intercepting, repeating, and fuzzing HTTP requests. |
| Wappalyzer | Identification of the technology stack (CMS, Frameworks). |
| `nmap` | Web server and version fingerprinting. |

## Associative Trails
Web application vulnerabilities are often the entry point for gaining an initial foothold in a corporate network, leading to internal pivoting.

## Connections
- [[Web Application Security (MOC)]]
- [[WAF Bypass Techniques]]

## Sources
- [[PEN-200-notes.md]]
