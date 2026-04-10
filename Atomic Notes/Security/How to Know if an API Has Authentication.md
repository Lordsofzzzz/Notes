---
status: seedling
tags: [security, api, authentication]
created: 2026-04-10
updated: 2026-04-10
---

# How to Know if an API Has Authentication

## Summary
The process of identifying and fingerprinting the authentication layers protecting an API to determine if security controls are correctly enforced.

## Details
An API may appear protected on some endpoints while leaving others (old versions, internal routes) wide open.

### Authentication Identification
Analyze request headers and cookies for common identifiers:
- `Authorization: Bearer <JWT>`
- `Authorization: Basic <base64>`
- `Authorization: ApiKey <key>`
- `X-API-Key: <custom_key>`
- `Cookie: session=abc123` or `Cookie: token=xyz789`

### Test Systematically (The "Remove & Retry" Test)
1. **With Credentials**: `curl <url> -H "Authorization: Bearer <token>"` -> Expected `200 OK`.
2. **Without Credentials**: `curl <url>` ->
   - **401 Unauthorized**: Correctly enforced.
   - **403 Forbidden**: Auth exists but lacks permissions.
   - **200 OK**: **CRITICAL FINDING** (Unauthenticated access).

### Auth Bypass Techniques (Path Manipulation)
If an endpoint returns `401`, try path traversal to trick the auth middleware:
- **Case Variation**: `/API/admin`
- **Dot Traversal**: `/api/./admin` or `/api/admin/.`
- **Double Slashes**: `/api//admin`
- **Semicolon Trick**: `/api/admin;test`
- **Versioning**: Test older, unauthenticated versions like `/api/v1/users` or `/api/v0/users`.

### Authentication Token Integrity
Test if the token is actually validated by the server:
- **Remove Signature**: Change a JWT to `alg: none` and remove the signature.
- **Modify Payload**: Change the `userId` in the payload without re-signing.
- **Brute Force Secret**: Use `hashcat -a 0 -m 16500 <jwt> wordlist.txt` if a weak secret is suspected.

## Associative Trails
Authentication is the first door. If it is weak or misconfigured, it renders all other security layers (like WAF) irrelevant. This note serves as the entry-point methodology for identifying those weak doors.

## Connections
- [[API Endpoint Testing]]
- [[What Leads to API Vulnerabilities]]

## Sources
- redteam-complete-guide.md
