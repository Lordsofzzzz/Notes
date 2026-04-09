---
status: seedling
tags: [red-team, waf, bypass, security]
created: 2026-04-10
updated: 2026-04-10
---

# WAF Bypass Techniques

## Summary
Methods and strategies used to evade Web Application Firewalls (WAF) during security engagements.

## Details
WAF Bypass involves various methods to evade Web Application Firewalls.

### Reconnaissance
Identify the WAF vendor using `wafw00f`, response headers, and error pages. Finding the origin IP via Shodan, Censys, or DNS history allows hitting the backend directly, rendering the WAF irrelevant.

### Encoding & Obfuscation
- **URL/Unicode Encoding**: Double encoding or Unicode can hide payloads.
- **Case Variation**: Using `SeLeCt` instead of `SELECT`.
- **Comment Injection**: `SEL/**/ECT` in SQL.
- **Null Bytes/Whitespace**: Using tabs or newlines to break pattern matching.

### HTTP-Level Tricks
- **Method Change**: Switching between POST and GET.
- **Chunked Encoding**: Fragmenting payloads at the transport layer.
- **Header Manipulation**: Spoofing `X-Forwarded-For` or `X-Real-IP` if trusted by the WAF.

### Payload Fragmentation
- **HTTP Parameter Pollution (HPP)**: Splitting payloads across multiple parameters.
- **Multipart Form Data**: Obscuring payloads within form boundaries.

### Tools
- **sqlmap**: Uses `--tamper` scripts.
- **Burp Suite**: Extensions like Bypass WAF.
- **nuclei**: WAF bypass templates.
- **wafw00f**: Fingerprinting the vendor.

## Associative Trails
Bypassing perimeter defense is the first step in most modern web engagements. This note exists to centralize bypass strategies and challenges the assumption that a WAF provides foolproof security. It refines the broader [[Red Team Methodology (Web Application)]].

## Connections
- [[Red Team Methodology (Web Application)]]
- [[Web Application Security (MOC)]]

## Sources
- [[redteam-complete-guide.md]]
- [[WAF Bypass Techniques.md]]
