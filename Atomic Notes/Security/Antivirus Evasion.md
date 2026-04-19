---
status: seedling
tags: [security, malware, defense-evasion]
created: 2026-04-19
updated: 2026-04-19
---

# Antivirus Evasion

## Summary
Techniques used to bypass Antivirus (AV) and Endpoint Detection and Response (EDR) solutions during the delivery and execution of malicious payloads.

## Details
**Core Logical Content Mandate**:
Modern security solutions use multiple detection layers. Evasion requires understanding these mechanisms to design payloads that remain undetected.

### AV Detection Methods
- **Signature-based (Static)**: Comparing file hashes or byte sequences against a database of known malware.
- **Heuristic / Behavioural**: Monitoring for suspicious activities (e.g., unexpected network connections or sensitive API calls).
- **Memory Scanning**: Scanning the RAM for malicious patterns or decrypted shellcode during execution.

### Bypass Techniques
| Technique | Type | Description |
|-----------|------|-------------|
| **On-disk obfuscation** | Static | Encrypting or packing the executable to change its signature. |
| **In-memory injection** | Runtime | Loading shellcode directly into memory without writing it to disk. |
| **Thread injection** | Runtime | Injecting code into the memory space of a legitimate process (Process Hollowing). |
| **LOLBins** | Runtime | "Living off the Land" - using trusted, pre-installed system binaries to execute malicious code. |

**Technical Verification**:
- Test payloads using tools like `VirusTotal` (with caution) or isolated VMs with Windows Defender enabled.
- Utilize custom loaders and shellcode encryption to defeat static signatures.

## Associative Trails
AV evasion is critical for maintaining a foothold on modern Windows systems where Defender or EDR solutions are ubiquitous.

## Connections
- [[Red Team Operational Security (OpSec)]]
- [[Windows Privilege Escalation]]

## Sources
- [[PEN-200-notes.md]]
