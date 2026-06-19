---
status: seedling
tags: [security, windows, vba, powershell, evasion]
created: 2026-04-19
updated: 2026-04-19
---

# Client-Side Code Execution

## Summary
Advanced techniques for executing malicious code on a client machine, focusing on in-memory runners and bypassing signature-based detection.

## Details
**Core Logical Content Mandate**:
Client-side code execution in advanced scenarios (PEN-300) focuses on bypassing modern defenses like AMSI and AV by avoiding disk writes.

### VBA Shellcode Runners
- **Win32 API Access**: VBA can call Win32 APIs directly to allocate memory and create threads.
- **Key APIs**:
    - `VirtualAlloc`: Allocate memory for shellcode.
    - `CreateThread`: Execute the shellcode in a new thread.
- **Evasion**: Use macro obfuscation and "VBA Stomping" (replacing source code while keeping compiled p-code) to evade static analysis.

### PowerShell Shellcode Runners
PowerShell can be used to load shellcode directly into memory without writing it to disk.
| Technique | Method |
|-----------|--------|
| **Add-Type** | Compiling C# code inline within a PowerShell session. |
| **Reflection** | Loading .NET assemblies directly into memory. |
| **DelegateType** | Calling Win32 APIs via delegates. |
| **UnsafeNativeMethods** | Using P/Invoke without explicit `[DllImport]` attributes. |

### JScript and Windows Script Host (WSH)
- **DotNetToJscript**: A technique to load .NET (C#) assemblies from JScript, allowing for fully in-memory execution via `mshta.exe` or `wscript.exe`.
- **MSHTA**: A common whitelisted binary used to execute JScript/VBScript via HTML Applications (.hta files), serving as an AppLocker bypass vector.

## Associative Trails
These techniques represent the transition from basic "macro phishing" to advanced "fileless" tradecraft used by modern red teams.

## Connections
- [[Antivirus Evasion]]
- [[AMSI Bypass]]
- [[AppLocker Bypasses]]

## Sources
- [[PEN-200-notes.md]] (PEN-300 Section)
