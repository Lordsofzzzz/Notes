---
status: seedling
tags: [security, api, enumeration]
created: 2026-04-10
updated: 2026-04-10
---

# Parameters in API Enumeration

## Summary
The systematic discovery and manipulation of input parameters in API endpoints to uncover hidden or undocumented functionality.

## Details
API vulnerabilities often hide in the parameters that developers didn't expect a user to control.

### 1. Finding Hidden Parameters
Identify undocumented inputs that change server behavior.
- **Tools**: Use `Arjun` (`arjun -u <url> -m JSON`) or the `Param Miner` Burp extension.
- **Wordlists**: Fuzz with `ffuf -w parameters.txt -u "https://target.com/api/users?FUZZ=test"`.
- **JS Crawling**: Extract strings from client-side JS: `grep -oE '[a-zA-Z_]+\s*[:=]\s*' app.js`.

### 2. Parameters to Prioritize
| Type | Example | Impact |
|---|---|---|
| **Debug/Internal** | `?debug=true`, `?admin=true`, `?internal=true` | Verbose errors, bypass validation |
| **Data Control** | `?limit=99999`, `?fields=*`, `?include=password` | Data theft, excessive disclosure |
| **Access Control** | `?user_id=1`, `?role=admin`, `?sudo=true` | Privilege escalation, IDOR |
| **Data Format** | `?format=xml`, `?callback=jsonp` | XXE, XSS |

### 3. Parameter Manipulation Attacks
- **Type Confusion**: Sending an array instead of a string: `{"username": ["admin"]}`.
- **Mass Assignment**: Injecting roles into body parameters: `{"username": "user", "role": "admin"}`.
- **Negative & Boundary Values**: `{"quantity": -1}` to test business logic or `{"price": 0}`.
- **Wildcard Injection**: Testing for SQLi (`'`), XSS (`<`), or NoSQLi (`$gt`) within parameter values.

## Associative Trails
A single undocumented parameter like `?debug=true` can leak an entire stack trace, exposing the underlying architecture. This note details the manual and automated "guessing" required to find those leaks.

## Connections
- [[API Endpoint Testing]]
- [[What Leads to API Vulnerabilities]]

## Sources
- redteam-complete-guide.md
