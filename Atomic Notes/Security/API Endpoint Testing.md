---
status: seedling
tags: [api, web-security, pen-testing, security]
created: 2026-04-10
updated: 2026-04-10
---

# API Endpoint Testing

## Summary
The process of discovering, mapping, and testing API endpoints for security vulnerabilities.

## Details
API Testing focuses on identifying and exploiting vulnerabilities in REST, GraphQL, or other web service interfaces.

### API Discovery
- **Crawl JS files**: Using `katana` or `linkfinder` to extract hidden endpoints.
- **Brute force**: Fuzzing with `ffuf` or `gobuster` for common paths (`/swagger.json`, `/v1/users`).
- **History**: Using `waybackurls` or `gau` to find old/deprecated versions.

### Common API Vulnerabilities
- **BOLA/IDOR (Broken Object Level Authorization)**: Accessing resources of other users by changing an ID.
- **Mass Assignment**: Over-assigning fields (e.g., sending `"role": "admin"`).
- **Broken Auth**: Bypassing authentication on old API versions or algorithm confusion for JWTs.
- **No Rate Limiting**: Allowing brute-force attacks on login or sensitive endpoints.
- **BFLA (Broken Function Level Authorization)**: Accessing administrative functions with lower-level privileges.

### Testing Steps
1. **Map Auth**: Check which endpoints require authentication.
2. **Version Fuzzing**: Check `v1`, `v2`, `v0`, `beta`, `dev` versions for forgotten security controls.
3. **HTTP Method Fuzzing**: Try `GET`, `POST`, `PUT`, `DELETE`, `PATCH`, `OPTIONS`, `TRACE` on every endpoint.
4. **Parameter Fuzzing**: Use `arjun` or `Param Miner` to find hidden parameters like `?debug=true` or `?admin=true`.

### Tools
- **Postman**: Manual testing.
- **Kiterunner**: Fast route discovery.
- **Arjun**: Hidden parameter discovery.
- **jwt_tool**: JWT manipulation.
- **Dalfox**: XSS scanning on API endpoints.
- **sqlmap**: SQL injection testing.

### Advanced API Attacks
- **GraphQL Introspection**: Extracting the full schema if introspection is enabled.
- **SSRF via Webhooks**: Using callback URLs to probe internal networks or cloud metadata.
- **Race Conditions**: Sending simultaneous requests to bypass logic (e.g., double spending).

## Associative Trails
APIs are the backbone of modern web apps and a primary attack surface for data breaches. This note exists to centralize discovery and exploitation techniques, challenging the common assumption that internal APIs don't need robust authentication.

## Connections
- [[Web Application Security (MOC)]]
- [[Web Application Security (MOC)]]

## Sources
- redteam-complete-guide.md
- API Endpoint Testing.md
