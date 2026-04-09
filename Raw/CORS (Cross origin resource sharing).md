## 1. What is CORS

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