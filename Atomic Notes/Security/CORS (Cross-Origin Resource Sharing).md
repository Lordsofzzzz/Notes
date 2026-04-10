---
status: seedling
tags: [web-security, cors, sop, security]
created: 2026-04-10
updated: 2026-04-10
---

# CORS (Cross-Origin Resource Sharing)

## Summary
A browser security mechanism that relaxes the Same-Origin Policy (SOP), allowing web pages to request resources from different origins.

## Details
CORS is purely a **browser-side enforcement**. If a server-to-server call (curl, Postman) is made, CORS does not apply.

### How CORS Works (Browser Flow)
1. **Request**: JS makes `fetch()` to `bank.com` with `Origin: evil.com`.
2. **Response**: `bank.com` responds with (or without) CORS headers.
3. **Enforcement**: Browser checks if `evil.com` is in `Access-Control-Allow-Origin`.
   - **No**: Browser blocks JS from reading the response.
   - **Yes**: Browser allows JS to read the response.

### Preflight Requests
Triggered for **Complex Requests** (PUT, DELETE, JSON, custom headers).
- **Simple Request (No Preflight)**: GET, POST, or HEAD with standard headers and `text/plain`, `multipart/form-data`, or `application/x-www-form-urlencoded`.
- **Preflight (OPTIONS)**: Browser asks the server if it's okay before sending the actual "dangerous" request.
```http
OPTIONS /data HTTP/1.1
Origin: https://app.com
Access-Control-Request-Method: DELETE
Access-Control-Request-Headers: Authorization
```

### Common Attack Vectors
- **Origin Reflection**: Server reflects any origin in `ACAO` header.
  ```http
  GET /api/data HTTP/1.1
  Origin: https://attacker.com
  -> Response: Access-Control-Allow-Origin: https://attacker.com
  -> Response: Access-Control-Allow-Credentials: true
  ```
- **Null Origin Bypass**: Whitelisting `null` origin, exploitable via sandboxed iframes.
  ```html
  <iframe sandbox="allow-scripts" src="data:text/html,<script>fetch('target.com/api', {credentials:'include'})</script>"></iframe>
  ```
- **Regex/Whitelist Bypass**:
  - `example.com` matches `evilexample.com`.
  - Prefix match `example.com` matches `example.com.evil.com`.
- **Preflight Bypass (Content-Type Trick)**: Sending JSON as `text/plain` to skip OPTIONS checks.

### Correct Implementation (Whitelist Approach)
```javascript
const ALLOWED_ORIGINS = ["https://app.example.com", "https://example.com"];
app.use((req, res, next) => {
  const origin = req.headers.origin;
  if (ALLOWED_ORIGINS.includes(origin)) {
    res.header("Access-Control-Allow-Origin", origin);
    res.header("Access-Control-Allow-Credentials", "true");
    res.header("Vary", "Origin"); // Essential for caching
  }
  next();
});
```

### Risk Table
| Finding | Risk |
|---|---|
| Wildcard + credentials | Critical |
| Origin reflection + credentials | Critical |
| Null origin trusted | High |
| HTTP origin whitelisted | Medium (MITM) |

## Associative Trails
CORS is one of the most commonly misconfigured security controls because developers often disable it or use wildcards to fix annoying browser errors during development. It represents a pivot from a public malicious page to sensitive authenticated data.

## Connections
- [[Web Application Security (MOC)]]
- [[Step-by-Step Web App Red Teaming]]

## Sources
- redteam-complete-guide.md
- CORS (Cross origin resource sharing).md

