

### 2. Intro to Cybersecurity
- CIA Triad — Confidentiality, Integrity, Availability
- Threat actors, CVEs, risk vs vulnerability
- Security controls — admin, technical, physical
- Shift-left security, threat modelling
- Laws, regulations, frameworks (NIST, ISO, etc.)



### 4. Report Writing
- Note portability — always write portable notes
- Pentest deliverables structure
- Effective screenshots
- Report sections:
  - Executive Summary
  - Testing Environment
  - Technical Summary
  - Findings + Recommendations
  - Appendices

---

## Enumeration & Recon

### Passive Info Gathering
| Tool | Use |
|------|-----|
| `whois` | domain registration data |
| Google dorks | `site:`, `filetype:`, `inurl:` |
| Netcraft | tech stack, hosting history |
| Shodan | internet-wide device search |
| GitHub recon | leaked keys, configs |
| Security Headers | SSL/TLS analysis |

### Active Info Gathering
| Tool | Use |
|------|-----|
| `nmap` | port scan, OS fingerprint, NSE scripts |
| SMB enum | shares, users, null sessions |
| SMTP enum | user enumeration via VRFY/EXPN |
| SNMP enum | community strings → configs |

### Vulnerability Scanning
- **Nessus** — credentialed + uncredentialed scans, plugin mgmt
- **Nmap NSE** — vuln scripts (`--script vuln`)

---

## Web Application Attacks

### Methodology
1. Fingerprint → 2. Enumerate → 3. Exploit → 4. Post-exploit

### Tools
| Tool | Use |
|------|-----|
| `gobuster` | dir brute force |
| Burp Suite | intercept, repeat, fuzz |
| Wappalyzer | tech stack ID |
| `nmap` | web server fingerprint |

### Vulnerabilities
- **XSS** — stored vs reflected, JS payload, priv esc via XSS
- **Directory Traversal** — `../` absolute vs relative paths, encoded `%2e%2e%2f`
- **LFI / RFI** — PHP wrappers (`php://filter`), remote inclusion
- **File Upload** — executable files, bypass via non-executable
- **Command Injection** — OS injection, `;`, `&&`, `|` chaining
- **SQLi** — error-based, UNION-based, blind, `sqlmap`

---

## Client-Side Attacks

- Target recon + fingerprinting
- **MS Office macros** — `.docm`, VBA, auto-run macros
- **Windows Library files** — `.library-ms` → UNC path → credential capture
- Social engineering delivery

---

## Antivirus Evasion

### AV Detection Methods
- Signature-based (static)
- Heuristic / behavioural
- Memory scanning

### Bypass Techniques
| Technique | Type |
|-----------|------|
| On-disk obfuscation | Static evasion |
| In-memory injection | Runtime evasion |
| Thread injection | Process hollowing variant |
| LOLBins | Living off the land |

- Test with: `VirusTotal`, isolated VM + Defender
- Tools: custom loaders, shellcode encryption

---

## Password Attacks

### Network Service Attacks
- SSH brute force — `hydra`, `medusa`
- RDP brute force
- HTTP POST login form

### Cracking
| Tool | Use |
|------|-----|
| `hashcat` | GPU cracking, rule-based |
| `john` | CPU, wordlist mutation |
| `rockyou.txt` | base wordlist |

- Wordlist mutation rules — `hashcat -r rules/`
- Crack methodology: hash ID → wordlist → rules → brute

### Hash-based Attacks (Windows)
| Attack | Tool | Notes |
|--------|------|-------|
| Crack NTLM | `hashcat -m 1000` | offline |
| Pass-the-Hash | `impacket`, `CME` | no plaintext needed |
| Crack Net-NTLMv2 | `hashcat -m 5600` | captured via Responder |
| Relay Net-NTLMv2 | `ntlmrelayx` | no cracking needed |

---

## Exploit Development / Fixing

- Buffer overflow — theory, overflow buffer adjustment
- Import + examine public exploits
- Cross-compile for target arch
- Fix web exploits — index out of range errors

### Public Exploit Sources
- Exploit-DB / `searchsploit`
- Packet Storm
- GitHub
- Google dorks for exploits
- Metasploit / `msfconsole`

---

## Windows Privilege Escalation

### Enumeration
- `winPEAS` — automated enum
- PowerShell — info goldmine (`Get-ChildItem`, history, env vars)
- `Sysinfo`, `whoami /priv`, `net user`

### Techniques
| Vector | Method |
|--------|--------|
| Service binary hijack | replace svc binary, restart |
| DLL hijacking | drop malicious DLL in search path |
| Unquoted service path | space in path → binary plant |
| Scheduled tasks | weak ACL on task/script |
| Token impersonation | `SeImpersonatePrivilege` |
| Kernel exploits | patch level check |

---

## Linux Privilege Escalation

### Enumeration
- `linPEAS` — automated
- SUID/SGID binaries — `find / -perm -4000`
- Capabilities — `getcap -r /`
- Cron jobs — `crontab -l`, `/etc/cron*`
- Writable `/etc/passwd`

### Techniques
| Vector | Method |
|--------|--------|
| SUID abuse | GTFObins |
| Sudo misconfig | `sudo -l` → exploit |
| Cron + writable script | inject reverse shell |
| NFS no_root_squash | mount + plant SUID |
| Capabilities | `cap_setuid+eip` → root |

---

## Port Redirection & Tunneling

- **SSH tunneling** — local, remote, dynamic (`-L`, `-R`, `-D`)
- **Chisel** — HTTP tunnel, SOCKS5
- **ligolo-ng** — modern pivot tool
- **Metasploit pivoting** — `route add`, SOCKS proxy

---

## Active Directory

### Enumeration
| Tool | Use |
|------|-----|
| `net` / `nltest` | legacy AD enum |
| PowerView | advanced AD enum |
| SharpHound | BloodHound data collector |
| BloodHound | attack path visualisation |

Key targets: Users, Groups, GPOs, SPNs, ACLs, Shares, Trusts

### Authentication Attacks
| Attack | Tool | Technique |
|--------|------|-----------|
| Password spray | `kerbrute`, CME | avoid lockout |
| AS-REP Roasting | `GetNPUsers.py` | no preauth required |
| Kerberoasting | `GetUserSPNs.py` | SPN → TGS → crack |
| Silver Ticket | `mimikatz` | forge service ticket |
| DCSync | `mimikatz` | replicate DC hashes |

### Lateral Movement
| Technique | Tool |
|-----------|------|
| WMI | `wmic`, `impacket` |
| WinRM | `evil-winrm` |
| PsExec | `impacket PsExec` |
| Pass-the-Hash | `CME`, `pth-winexe` |
| Overpass-the-Hash | `mimikatz` → TGT |
| Pass-the-Ticket | inject `.kirbi` |
| DCOM | `MMC20.Application` |

### Persistence
- **Golden Ticket** — forge TGT with KRBTGT hash (10yr validity)
- **Shadow Copies** — `vssadmin` → extract NTDS.dit

---

## Metasploit

- `msfconsole` → `search`, `use`, `set`, `run`
- Meterpreter — shell upgrade, file ops, pivot
- Post-exploitation modules
- **Exam rule** — Metasploit allowed on ONE machine only

---

## Assembling the Pieces (Full Attack Chain)

```
External recon (MAILSRV1, WEBSRV1)
    ↓
Initial foothold (phish / web exploit)
    ↓
Internal network access (pivot)
    ↓
Internal enumeration (BloodHound)
    ↓
Kerberoast / relay attack
    ↓
Domain Controller compromise
    ↓
DCSync → all hashes
```

---

## OSCP Exam Notes

- **24hr attack window** + 24hr report submission
- Standalone machines + AD set (mandatory)
- Metasploit — 1 machine only
- Report must include: steps, screenshots, proof.txt
- `local.txt` = low priv, `proof.txt` = root/SYSTEM

---

## Key Tools Quick Reference

```
Recon:        nmap, gobuster, theHarvester, shodan
Web:          burpsuite, sqlmap, nikto
Exploit:      searchsploit, metasploit, impacket
PrivEsc:      winPEAS, linPEAS, PowerView, BloodHound
Crack:        hashcat, john, hydra
Pivot:        chisel, ligolo-ng, ssh -D
AD:           mimikatz, rubeus, CrackMapExec, evil-winrm
C2:           meterpreter (exam: 1 machine only)
```

---

## Try Harder Mindset

- Enumerate more before guessing
- Document everything — notes = report
- If stuck: re-enumerate, different vector
- Low-hanging fruit first, then chain vulns
- Always screenshot proof files in context (`whoami && hostname && cat proof.txt`)

---

---

# PEN-300 / OSEP — Evasion Techniques & Breaching Defenses
> OffSec Advanced Evasion Course (2021) | 704 pages | OSEP prep

---

## Course Structure

```
Advanced evasion → AV/EDR bypass → C2 infra → 
AD exploitation → Forest attacks → Full chain
```

- Prerequisite: OSCP (PEN-200)
- Focus: **evading modern defenses**, not just exploitation
- Lab: simulated enterprise with AV, AppLocker, AMSI, proxies

---

## OS & Programming Fundamentals

- Win32 API theory — how userland calls kernel
- Windows on Windows (WoW64) — 32-bit on 64-bit
- Windows Registry — persistence, config abuse
- Programming language levels — compiled vs interpreted vs JIT
- C#, PowerShell, JScript, VBA — all used in course

---

## Client-Side Code Execution

### Office / VBA
- Staged vs non-staged payloads
- HTML Smuggling — bypass email gateways, deliver via browser
- VBA macros — shellcode runner directly in Word memory
- Calling Win32 APIs from VBA — `VirtualAlloc`, `CreateThread`
- Macro obfuscation — evade signature detection
- Word template stomping — `Normal.dotm` abuse

### PowerShell Shellcode Runners
| Technique | Method |
|-----------|--------|
| Add-Type | compile C# inline |
| Reflection | load assembly without disk write |
| DelegateType | call Win32 via delegate |
| UnsafeNativeMethods | P/Invoke without `[DllImport]` |

- Proxy-aware comms — `WebClient` with system proxy
- User-Agent spoofing — blend with browser traffic
- SYSTEM proxy — when running as SYSTEM, proxy config differs

### JScript / WSH
- `DotNetToJscript` — load C# from JScript, no disk write
- Shellcode runner in C# → wrapped in JScript
- `SharpShooter` — automated payload generation
- MSHTA + JScript — AppLocker bypass vector
- XSL Transform — `wmic` + XSL → code exec

---

## Process Injection & Migration

### Injection Types
| Technique | Description |
|-----------|-------------|
| Classic injection | `VirtualAllocEx` + `WriteProcessMemory` + `CreateRemoteThread` |
| DLL injection | load malicious DLL into target process |
| Reflective DLL injection | DLL loads itself — no `LoadLibrary` call |
| Process hollowing | spawn suspended process, replace image |

- Target legit processes: `explorer.exe`, `svchost.exe`, `teams.exe`
- Reflective DLL in PowerShell — fully in-memory

---

## Antivirus Evasion (Deep)

### Detection Layers
- Static signature scanning
- Behavioural emulation
- Memory scanning at runtime
- ETW (Event Tracing for Windows)

### Bypass Techniques
| Technique | Effect |
|-----------|--------|
| Sleep timers | defeat emulator (emulators skip long sleeps) |
| Non-emulated APIs | call APIs emulator doesn't support |
| Shellcode encryption (XOR/AES) | defeat static scan |
| In-memory decryption | never write plaintext to disk |
| VBA stomping | replace macro source, keep p-code |
| WMI dechaining | break parent-child process link |
| VBA obfuscation | string splitting, Chr() encoding |

- Test environment: local VM + same AV as target
- Locate signatures: binary search + `msfvenom` + AV scan loop

---

## AMSI Bypass

- AMSI = Antimalware Scan Interface — hooks PS, JScript, .NET
- Detection flow: `AmsiInitialize` → `AmsiScanBuffer` → result
- Bypass methods:
  - Patch `AmsiScanBuffer` return value in memory
  - Corrupt `amsiContext` pointer
  - Reflection to force `amsiInitFailed`
  - JScript-specific bypass via registry key
- Post-bypass: PowerShell runs without AMSI scanning

---

## Application Whitelisting (AppLocker)

### Bypasses
| Method | Technique |
|--------|-----------|
| Trusted folders | write to `C:\Windows\Tasks` etc. |
| DLL bypass | AppLocker default: DLLs not blocked |
| Third-party execution | use allowed binaries as launchers |
| PowerShell CLM bypass | custom runspace outside PS host |
| C# bypass | reverse-engineer allowed app, inject shellcode |
| JScript + MSHTA | `mshta.exe` often whitelisted |
| XSL + WMIC | `wmic.exe` + XSL file = code exec |
| Alternate Data Streams | hide payload in NTFS ADS |

- PowerShell Constrained Language Mode (CLM) — blocked by AppLocker
- Custom runspaces bypass CLM — call PowerShell API directly from C#

---

## Network Filter Bypasses

| Filter | Bypass |
|--------|--------|
| DNS filter | DNS tunneling (`dnscat2`) |
| Web proxy | proxy-aware payloads, CONNECT method |
| IDS/IPS | encrypt C2 traffic, blend with legit protocols |
| HTTPS inspection | custom certs, domain fronting |
| Full packet capture | TLS + domain fronting |

### Domain Fronting
- Route C2 traffic via CDN (Azure, Cloudflare, AWS)
- HTTP `Host` header → real C2, SNI → legit CDN domain
- CDN forwards traffic internally
- Network sees: `*.azureedge.net` — not your C2

### DNS Tunneling
- `dnscat2` — C2 channel over DNS queries
- Useful when only DNS egress allowed
- Slow but stealthy

---

## Linux Post-Exploitation

- User config file backdoors (`.bashrc`, `.profile`)
- VIM config — keylogger, reverse shell on vim open
- Shared library hijacking — `LD_LIBRARY_PATH`, `LD_PRELOAD`
- Kerberos on Linux — steal `.keytab` files, ccache files
- Use `impacket` with stolen Kerberos tickets

---

## Kiosk Breakouts

- Browser enumeration — find allowed URLs, protocols
- Filesystem access via browser dialogs
- Firefox profile abuse — extract saved creds, cookies
- Privilege escalation from kiosk → full shell
- Cron job abuse — root shell at scheduled time
- Windows kiosk — sticky keys, accessibility tool abuse

---

## Windows Credentials

### Local
- SAM database — `reg save HKLM\SAM`
- Access token impersonation — `SeImpersonatePrivilege`
- Token theft with `Incognito` / `mimikatz token::elevate`

### Domain
- `mimikatz` — `sekurlsa::logonpasswords`, `lsadump::dcsync`
- Memory dump — `MiniDumpWriteDump` → process offline with `mimikatz`
- Kerberos ticket extraction — `.kirbi` files

---

## Windows Lateral Movement

### RDP
- `xfreerdp`, `rdesktop` — standard RDP
- Reverse RDP proxy — `Metasploit`, `Chisel` tunnel
- Credential theft from RDP sessions — `tscon` session hijack
- RDP as console — `mstsc /admin`

### Fileless Lateral Movement
- Auth + execution without touching disk
- WMI, WinRM, DCOM in C# — fully in-memory execution

---

## Linux Lateral Movement

### SSH
- Key harvesting — `~/.ssh/id_rsa`, `authorized_keys`
- SSH persistence — add key to `authorized_keys`
- SSH ControlMaster hijacking — hijack existing multiplexed session
- SSH-Agent forwarding abuse — use victim's agent to hop further

### DevOps Targets
| Tool | Attack |
|------|--------|
| Ansible | enumerate playbooks, ad-hoc commands, cred leakage |
| Artifactory | backup files, DB access, add admin account |

---

## Microsoft SQL Server Attacks

- MS SQL in AD — often runs as domain service account
- **UNC path injection** — trigger NTLM auth to attacker
- **Hash relay** — capture + relay MSSQL service account hash
- **Privilege escalation** — `EXECUTE AS LOGIN`, `xp_cmdshell`
- **Linked servers** — pivot across SQL servers across domains
- Custom assemblies — load C# DLL into SQL → code exec

---

## Active Directory Exploitation (Advanced)

### Object Permissions Abuse
| Permission | Abuse |
|------------|-------|
| `GenericAll` | full control → reset password, add to group |
| `WriteDACL` | add permissions to any object |
| `GenericWrite` | write attributes → targeted Kerberoast |
| `ForceChangePassword` | reset without knowing current pass |

### Kerberos Delegation
| Type | Attack |
|------|--------|
| Unconstrained | steal TGT from any authenticating user |
| Constrained | S4U2Self/S4U2Proxy → impersonate users |
| Resource-based (RBCD) | attacker controls target's `msDS-AllowedToActOnBehalfOfOtherIdentity` |

### Forest Attacks
- **SID History abuse** — inject ExtraSID from trusted forest → cross-forest priv esc
- **Forest trust enumeration** — `nltest`, PowerView `Get-DomainTrust`
- **Printer bug (SpoolSample)** — force DC to auth to attacker → unconstrained delegation + TGT
- **Cross-forest compromise** — ExtraSID + forged inter-realm TGT
- **Linked SQL servers across forests** — chain SQL hops across trust boundaries

---

## Full Attack Chain (PEN-300)

```
Phish → Office macro / HTML smuggle
    ↓
Shellcode in memory (no disk write)
    ↓
AMSI bypass + AV evasion
    ↓
C2 via domain fronting / DNS tunnel
    ↓
Local priv esc (token impersonation)
    ↓
Credential dump (mimikatz / memory)
    ↓
Lateral movement (fileless WMI/WinRM)
    ↓
MSSQL pivot or Kerberos delegation abuse
    ↓
Cross-domain / cross-forest
    ↓
DA → EA (Enterprise Admin)
```

---

## OSEP Exam Notes

- 48hr attack window (vs OSCP's 24hr)
- Full simulated enterprise network
- Must compromise specific objectives (not just root every box)
- No partial credit — objectives or nothing
- Report due 24hr after exam ends
- Metasploit usage — check current exam guide

---

## PEN-300 Key Tools

```
Payload gen:    msfvenom, SharpShooter, DotNetToJscript
Evasion:        custom C# loaders, VBA obfuscation, AMSI patch
Injection:      reflective DLL, process hollowing
C2:             Cobalt Strike / Sliver + domain fronting
Network:        dnscat2, Chisel, domain fronting
Creds:          mimikatz, MiniDumpWriteDump, impacket
AD:             BloodHound, PowerView, Rubeus
MSSQL:          PowerUpSQL, impacket mssqlclient
Linux:          ssh-agent hijack, LD_PRELOAD, keytab theft
```

---

## PEN-200 vs PEN-300 Delta

| Area | PEN-200 | PEN-300 |
|------|---------|---------|
| AV evasion | basic bypass | deep — encrypt, obfuscate, stomp |
| Payloads | msfvenom | custom C#/VBA/JScript loaders |
| Network | no filter bypass | domain fronting, DNS tunnel, proxy bypass |
| AD | basic attacks | delegation, forest trusts, SID abuse |
| Lateral | standard tools | fileless, SSH hijack, DevOps |
| MSSQL | not covered | full attack chain |
| Linux | basic | kiosk, shared libs, Kerberos, DevOps |
