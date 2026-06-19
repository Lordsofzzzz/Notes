---
status: seedling
tags: [security, password-cracking, authentication]
created: 2026-04-19
updated: 2026-04-19
---

# Password Attacks

## Summary
Techniques and tools for bypassing authentication via network service brute forcing, offline hash cracking, and Windows-specific hash attacks.

## Details
**Core Logical Content Mandate**:
Password attacks are divided into network-based (online) and hash-based (offline) methods. Online attacks involve interacting with a live service, while offline attacks involve cracking captured credentials without alerting the target system.

### Network Service Attacks
Involves brute-forcing protocols like SSH, RDP, or HTTP login forms.
- **Tools**: `hydra`, `medusa`.
- **Methodology**: Target service -> Wordlist -> Brute force attempt.

### Offline Hash Cracking
Cracking cryptographic hashes using CPU or GPU power.
| Tool | Use |
|------|-----|
| `hashcat` | GPU-optimized cracking, supports complex rule-based attacks. |
| `john` | CPU-based cracking, excellent for wordlist mutation. |
| `rockyou.txt` | The industry standard base wordlist for cracking. |

**Cracking Methodology**:
1. Identify the hash type (e.g., MD5, SHA256, NTLM).
2. Select an appropriate wordlist (`rockyou.txt`).
3. Apply mutation rules if necessary (`hashcat -r rules/`).
4. Execute brute force/dictionary attack.

### Windows Hash-based Attacks
Windows environments use NTLM and Net-NTLMv2 hashes which enable specific attack vectors.
| Attack | Tool | Notes |
|--------|------|-------|
| Crack NTLM | `hashcat -m 1000` | Offline cracking of captured SAM/NTDS.dit hashes. |
| Pass-the-Hash | `impacket`, `CME` | Authenticating using the NTLM hash directly; no plaintext needed. |
| Crack Net-NTLMv2 | `hashcat -m 5600` | Captured over the network via tools like `Responder`. |
| Relay Net-NTLMv2 | `ntlmrelayx` | Relaying captured authentication to another machine (SMB signing must be off). |

## Associative Trails
This note centralizes the password-related techniques learned during the PEN-200 course, bridging the gap between initial enumeration and gaining a foothold or escalating privileges.

## Connections
- [[Red Team Tools]]
- [[Active Directory Authentication Attacks]]

## Sources
- [[PEN-200-notes.md]]
