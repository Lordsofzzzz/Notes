---
status: seedling
tags: [security, windows, applocker, defense-evasion]
created: 2026-04-19
updated: 2026-04-19
---

# AppLocker Bypasses

## Summary
Methods for executing unauthorized code on systems where AppLocker or other Application Whitelisting (AWL) solutions are enforced.

## Details
**Core Logical Content Mandate**:
AppLocker bypasses leverage trusted system binaries, folders, or inherent flaws in the policy enforcement.

### Common Bypass Techniques
| Method | Technique |
|--------|-----------|
| **Trusted Folders** | Writing payloads to directories like `C:\Windows\Tasks` or `C:\Windows\Temp` which may have default "Allow" rules. |
| **DLL Bypass** | AppLocker policies often target executables (`.exe`) but may not enforce rules on DLLs, allowing for reflective loading. |
| **Living off the Land (LOLBins)** | Using allowed binaries like `mshta.exe`, `wmic.exe`, or `msbuild.exe` to execute arbitrary code. |
| **PowerShell CLM Bypass** | Constrained Language Mode (CLM) is often enabled by AppLocker. It can be bypassed by creating a custom PowerShell runspace in a C# application. |
| **XSL Transform** | Using `wmic.exe` with a remote or local XSL file to execute JScript/VBScript code. |

### Technical Logic
1. **Identify Policy**: Enumerate which paths and binaries are whitelisted.
2. **Select Vector**: Choose an allowed binary that supports code execution (e.g., `mshta`).
3. **Execute**: Launch the payload through the trusted proxy.

## Associative Trails
AppLocker is a significant hurdle for attackers; bypassing it is essential for executing custom toolsets in hardened environments.

## Connections
- [[Client-Side Code Execution]]
- [[AMSI Bypass]]

## Sources
- [[PEN-200-notes.md]] (PEN-300 Section)
