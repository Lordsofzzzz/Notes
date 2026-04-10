---
status: sapling
tags: [security, red-team, tools]
created: 2026-04-10
updated: 2026-04-10
---

# Red Team Tools

## Summary
A comprehensive reference for the red team toolkit, categorized by the phase of the assessment from reconnaissance to post-exploitation.

## Details
### Reconnaissance & Information Gathering
- **Subdomain Discovery**:
  - `subfinder -d target.com`: Fast, passive subdomain enumeration.
  - `amass enum -d target.com`: Comprehensive discovery using multiple data sources.
  - `assetfinder target.com`: Quick discovery via certificates and search engines.
- **URL & Endpoint Discovery**:
  - `waybackurls target.com`: Extracts URLs from the Wayback Machine.
  - `gau target.com`: Fetches URLs from AlienVault Open Threat Exchange, Wayback Machine, and Common Crawl.
  - `katana -u https://target.com -jc`: Modern web crawler for endpoint discovery including JS files.
- **Technology Fingerprinting**:
  - `whatweb https://target.com`: Identifies technologies, versions, and headers.
  - `httpx -tech-detect`: Fast HTTP probe with technology detection.

### Scanning & Fuzzing
- **Port Scanning**:
  - `nmap -sV -sC -p- target.com`: Service and script scanning on all ports.
- **Directory & Parameter Bruteforce**:
  - `ffuf -w wordlist.txt -u https://target.com/FUZZ`: High-speed fuzzer for directories and files.
  - `feroxbuster -u https://target.com`: Recursive path discovery.
  - `arjun -u https://target.com/api/endpoint`: Discovery of hidden HTTP parameters.
- **Vulnerability Scanning**:
  - `nuclei -t vulnerabilities/`: Template-based vulnerability scanner for known CVEs and misconfigurations.
  - `nikto -h https://target.com`: General web server vulnerability scanner.

### Exploitation & Interception
- **Interception Proxy**:
  - **Burp Suite Professional**: The industry standard for manual web testing.
    - Essential Extensions: *Autorize* (Auth testing), *JWT Editor*, *Param Miner*, *Turbo Intruder* (Race conditions).
- **Injection Tools**:
  - `sqlmap -u "URL" --dbs`: Automated SQL injection and database takeover.
  - `commix`: Automated command injection exploitation.
  - `dalfox`: Advanced XSS scanner and parameter analyzer.
- **Authentication Attacks**:
  - `jwt_tool.py`: Comprehensive toolkit for JWT tampering and algorithm confusion.
  - `hydra`: Fast network login cracker for various protocols.

### Advanced Attacks
- **SSRF & Out-of-Band (OOB)**:
  - `interactsh-client`: Client for the Interactsh OOB interaction server.
  - `ssrfmap`: Automated SSRF discovery and exploitation.
- **GraphQL**:
  - `graphw00f.py`: GraphQL fingerprinting and introspection tool.

### Tool Selection by Experience
| Level | Recommended Tools |
|---|---|
| Beginner | Burp Suite, Nmap, SQLMap, Nikto, FFUF |
| Intermediate | Nuclei, Subfinder, JWT Tool, Dalfox, Httpx |
| Advanced | Katana, Kiterunner, SSRFmap, Custom automation |

## Associative Trails
This note exists to centralize the toolkit references, providing a quick lookup for the software required during different phases of the methodology. It maps directly to the operational phases described in [[Step-by-Step Web App Red Teaming]].

## Connections
- [[Web Application Security (MOC)]]
- [[WAF Bypass Techniques]]
- [[Step-by-Step Web App Red Teaming]]
- [[API Endpoint Testing]]

## Sources
- [[redteam-complete-guide.md]]
