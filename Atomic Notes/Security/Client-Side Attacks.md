---
status: seedling
tags: [security, windows, delivery, phishing]
created: 2026-04-19
updated: 2026-04-19
---

# Client-Side Attacks

## Summary
Attacks that target the user's applications and environment, typically used as an initial access vector through social engineering.

## Details
**Core Logical Content Mandate**:
Client-side attacks rely on delivering a malicious file or link that the target executes within their own environment.

### Delivery Mechanisms
- **MS Office Macros**:
    - File Types: `.docm` (macro-enabled Word document).
    - Execution: VBA (Visual Basic for Applications) code that runs upon the document being opened (if macros are enabled).
    - Goal: Execution of shellcode or a reverse shell downloader.
- **Windows Library Files (`.library-ms`)**:
    - Vector: Delivering a `.library-ms` file that points to a UNC path.
    - Result: Capturing Net-NTLMv2 hashes when the user browses the library, or executing code if the user interacts with the remote share.

### Execution Logic
1. **Target Recon**: Identify software used by the target (e.g., Office versions).
2. **Payload Generation**: Create a malicious document or library file.
3. **Social Engineering**: Deliver the payload via email or other communication channels.
4. **Credential/Session Capture**: Use `Responder` or listeners to capture hashes or receive a shell.

## Associative Trails
Client-side attacks bypass traditional perimeter defenses by executing "inside-out," leveraging the user's trusted environment.

## Connections
- [[Antivirus Evasion]]
- [[Red Team Operational Security (OpSec)]]

## Sources
- [[PEN-200-notes.md]]
