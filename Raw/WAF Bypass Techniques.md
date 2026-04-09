
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
