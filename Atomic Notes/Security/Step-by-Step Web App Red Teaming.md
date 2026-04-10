---
status: seedling
tags: [security, red-team, methodology]
created: 2026-04-10
updated: 2026-04-10
---

# Step-by-Step Web App Red Teaming

## Summary
A comprehensive, 10-phase execution guide for conducting professional red team assessments of web applications.

## Details
This methodology ensures a consistent and high-impact approach across every engagement.

### Phase 1: Passive & Active Recon
Identify assets and technology. Use `subfinder`, `amass`, `httpx`, and Google dorks (`site:target.com inurl:admin`).

### Phase 2: Authentication Testing
Identify and bypass login mechanisms.
- Default credentials, username enumeration, and brute force (no rate limit?).
- Password reset flaws (tokens in URL, predictable secrets).
- JWT misconfigurations (algorithm confusion, tampering).

### Phase 3: Authorization (BOLA/IDOR)
Test if users can access other users' data.
- **BOLA**: Incrementing IDs in API calls.
- **Privilege Escalation**: Elevate from `user` to `admin` via roles or `isAdmin` flags.

### Phase 4: Injection Testing
Manual and automated validation of inputs.
- **SQLi**: `sqlmap -u <url> --dbs`.
- **XSS**: Reflected and stored payloads in search/profile fields.
- **SSTI**: `{{7*7}}` for template engine detection.

### Phase 5: Application Logic
Test for business-specific flaws.
- Negative quantities/prices in e-commerce.
- Step-bypass in multi-step workflows.
- Race conditions (simultaneous balance withdrawals).

### Phase 6: Infrastructure & Misconfiguration
- Missing security headers (`X-Frame-Options`, `CSP`).
- Exposed files (`.env`, `.git/config`, `actuator/env`).
- Outdated software versions.

### Phase 7: CORS, CSRF, Clickjacking
Test browser-based access controls.
- CORS origin reflection and credentials theft.
- CSRF token validation (missing or weak tokens?).
- Frame-ancestors (clickjacking).

### Phase 8: File Uploads
Test for Remote Code Execution (RCE).
- Bypass extensions (`.php.jpg`), MIME types, and magic bytes.
- Path traversal in filenames.

### Phase 9: API-Specific Testing
Test API versioning (`v1` vs `v3`), HTTP method tampering (`DELETE` via `X-HTTP-Method-Override`), and Mass Assignment.

### Phase 10: Advanced SSRF & XXE
Probing internal networks and cloud metadata.
- **SSRF**: `?url=http://169.254.169.254/latest/meta-data/`.
- **XXE**: Injecting entities to read local files (`/etc/passwd`).

### Priority Order for Maximum Impact
1. **Authentication Bypass** (Full Access)
2. **IDOR/BOLA** (Data Theft)
3. **SQLi** (Database Dump)
4. **SSRF** (Internal Access)
5. **Stored XSS** (Scaling Attacks)

## Associative Trails
A phased methodology is critical for ensuring no common bug class is missed. This note organizes the entire vault's security knowledge into a professional workflow.

## Connections
- [[Web Application Security (MOC)]]
- [[API Endpoint Testing]]
- [[Red Team Tools]]

## Sources
- redteam-complete-guide.md
