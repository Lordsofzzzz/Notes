---
status: seedling
tags: [web-security, cors, sop, security]
created: 2026-04-10
updated: 2026-04-10
---

# CORS (Cross-Origin Resource Sharing)

## Summary
A browser-side security mechanism that relaxes the Same-Origin Policy (SOP) to allow cross-origin requests.

## Details
CORS is a browser-side security mechanism that relaxes the Same-Origin Policy (SOP). It controls how web pages request resources from a different origin (Protocol + Domain + Port).

### How CORS Works
1. **Request**: Browser sends `Origin: source.com`.
2. **Response**: Server returns `Access-Control-Allow-Origin: source.com`.
3. **Enforcement**: The **browser** (not the server) checks the header and blocks or allows the response.

### Key Headers
- `Access-Control-Allow-Origin`: Specifies which origins are allowed.
- `Access-Control-Allow-Credentials`: If true, allows cookies to be sent with cross-origin requests.
- `Vary: Origin`: Crucial for caching to prevent serving the wrong CORS policy to a different origin.

### Preflight Requests
Triggered for "complex" requests (PUT, DELETE, JSON content, or custom headers). The browser sends an `OPTIONS` request first to ask the server for permission.

### Common Misconfigurations
- **Wildcard with Credentials**: `ACAO: *` + `Credentials: true` is forbidden by browsers, but reflecting any origin is a critical risk.
- **Origin Reflection**: Dynamically echoing the `Origin` header from the request without a whitelist.
- **Null Origin Trust**: Whitelisting `null` allows sandboxed iframes to steal data.
- **Substring Match**: Using loose regexes like `example.com` which matches `evil-example.com`.

### Impact of Misconfigured CORS
An attacker can host a malicious site that uses the victim's session to steal data from a vulnerable API or perform actions on their behalf.

### Tools
- **CORScanner**: Automated misconfiguration scanner.
- **Burp Suite**: Manual testing of `Origin` headers.
- **corsy**: Python tool for detection.

## Associative Trails
CORS was designed for sharing, but its misconfiguration is a top-tier vulnerability for cross-site data theft. This note refines the understanding of Same-Origin Policy (SOP) and clarifies that CORS is enforced by the client browser, not the server.

## Connections
- [[Web Application Security (MOC)]]
- [[Web Application Security (MOC)]]

## Sources
- redteam-complete-guide.md
- CORS (Cross origin resource sharing).md
