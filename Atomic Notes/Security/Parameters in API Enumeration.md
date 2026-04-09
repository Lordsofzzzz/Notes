---
status: seedling
tags: [security, api, enumeration]
created: 2026-04-10
updated: 2026-04-10
---

# Parameters in API Enumeration

## Summary
Techniques for identifying and testing input parameters in API endpoints to uncover hidden functionality.

## Details
- **Fuzzing Parameters**: Using lists like `SecLists` with `ffuf` to find undocumented inputs.
- **Type Juggling**: Testing how the API handles different data types (string vs. int vs. array).
- **HTTP Verb Tampering**: Testing `PUT`, `DELETE`, and `PATCH` on endpoints that typically use `GET` or `POST`.

## Associative Trails
API vulnerabilities often hide in the parameters that developers didn't expect users to modify. This note focuses on the "how" of discovery.

## Connections
- [[API Endpoint Testing]]
- [[Web Application Security (MOC)]]

## Sources
- redteam-complete-guide.md
