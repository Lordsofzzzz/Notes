---
status: seedling
tags: [security, api, vulnerabilities]
created: 2026-04-10
updated: 2026-04-10
---

# What Leads to API Vulnerabilities

## Summary
The root causes of API security failures, focusing on development trade-offs and architectural oversight.

## Details
Understanding *why* APIs fail allows for predictive testing and high-impact discovery.

### 1. Speed Over Security
Fast development cycles (`ship fast, fix later`) often result in:
- **Authentication Bolted-On**: Security middleware only added to some routes (`/api/payment` but not `/api/users`).
- **Abandoned Endpoints**: Forgotten, unauthenticated versions (`/api/v1/`) left running.
- **Missing Rate Limits**: Assumed to be a "future" requirement.

### 2. Client-Side Over-Reliance
Developers often trust the client application, leading to:
- **Zero Backend Validation**: Assuming the frontend only sends valid integers or strings.
- **Over-Returning Data**: API returns the full database object (e.g., including `password_hash`, `secret_key`), assuming the frontend will filter it.

### 3. Identity & Trust Issues
- **Trusting HTTP Headers**: Relying on client-controlled headers like `X-User-ID` or `X-Forwarded-For` for authentication or routing.
- **No Internal/External Separation**: Exposing admin or internal APIs (`/api/internal/stats`) on the same public-facing server.

### 4. Logic & Authorization Complexity
- **Broken Object Level Authorization (BOLA)**: Complexity in user-to-resource mapping leads to guessing IDs.
- **Mass Assignment**: Frameworks (like Rails or Spring) often bind incoming JSON to models by default, allowing attackers to over-assign sensitive fields like `role=admin`.

## Associative Trails
Vulnerabilities are rarely random; they are symptoms of the underlying development culture. This note provides the context for the technical testing described in [[API Endpoint Testing]].

## Connections
- [[API Endpoint Testing]]
- [[Parameters in API Enumeration]]
- [[How to Know if an API Has Authentication]]

## Sources
- redteam-complete-guide.md
