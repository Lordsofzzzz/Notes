---
status: seedling
tags: [red-team, waf, bypass, security]
created: 2026-04-10
updated: 2026-04-10
---

# WAF Bypass Techniques

## Summary
The systematic strategy for identifying and evading Web Application Firewall (WAF) protections to reach the underlying application origin.

## Details
A WAF acts as an application-level perimeter defense. Bypassing it involves several distinct technical strategies.

### 1. Direct Origin IP Identification
If the attacker can find the origin IP, they can connect directly, bypassing the WAF (e.g., Shodan, Censys, SSL cert logs, historical DNS).

### 2. Encoding & Obfuscation
Hiding malicious payloads within formats the WAF might not decode correctly.
- **URL/Unicode/Double Encoding**: `%2553%2545%254C%2545%2543%2554` for `SELECT`.
- **Case Variation**: `SeLeCt * FrOm users;`.
- **Comment Injection**: `SEL/**/ECT` or `UNION/*--*/*/SELECT`.
- **Null Byte/Whitespace**: `SELECT%00*` or using tabs/newlines (`%09`, `%0a`) to break pattern matching.

### 3. HTTP-Level Tricks
- **Method Change**: Some WAFs only inspect POST; try GET instead (or vice versa).
- **Chunked Transfer Encoding**: Fragment the payload at the HTTP layer to bypass inspection.
- **Header Manipulation**: Manipulate `X-Forwarded-For`, `X-Real-IP`, or `X-Originating-IP` if the WAF trusts these as internal traffic.
- **IP Rotation**: Use tools like `ffuf` or `wfuzz` to rotate IPs or user agents to bypass rate/timing thresholds.

### 4. Payload Fragmentation & Pollution
- **HTTP Parameter Pollution (HPP)**: Splitting a payload across multiple identical parameters.
- **Multipart Form Data**: Wrapping payloads in complex boundary structures that are difficult for some WAFs to parse.

### 5. Protocol & Path Confusion
- **Path Traversal Variations**: `/app/./admin`, adding extra slashes `//admin`, or `admin;test`.
- **Case Sensitivity**: Exploiting differences between the WAF (case-sensitive) and the backend (case-insensitive).

### Tools and Automation
| Tool | Purpose | Key Flag/Technique |
|---|---|---|
| **wafw00f** | Fingerprinting | Identifies the WAF vendor and response headers. |
| **sqlmap** | SQLi Bypass | `--tamper=space2comment,randomcase` |
| **nuclei** | Vulnerability Scan | Use WAF bypass templates. |
| **Burp Suite** | Manual Manipulation | Use Bypass WAF or HackBar extensions. |

## Associative Trails
A WAF is a "Security Guard at the door," but the real security is the application code. This note documents the technical methods to prove that perimeter defense is not a substitute for secure coding.

## Connections
- [[Web Application Security (MOC)]]
- [[Step-by-Step Web App Red Teaming]]

## Sources
- redteam-complete-guide.md
- WAF Bypass Techniques.md
