---
status: seedling
tags: [security, windows, amsi, evasion]
created: 2026-04-19
updated: 2026-04-19
---

# AMSI Bypass

## Summary
Techniques to disable or circumvent the Antimalware Scan Interface (AMSI) on Windows, which monitors PowerShell, JScript, and .NET execution.

## Details
**Core Logical Content Mandate**:
AMSI acts as a bridge between scripting engines and installed AV/EDR solutions. Bypassing it allows malicious scripts to run without being scanned.

### Detection Mechanism
1. Scripting engine (e.g., `powershell.exe`) calls `AmsiInitialize`.
2. Script buffer is sent to `AmsiScanBuffer`.
3. AMSI returns a result (1 = Clean, 32768 = Malicious).

### Bypass Methods
- **Memory Patching**: Patching the `AmsiScanBuffer` function in the current process's memory to always return a "Clean" result (0) or an error.
- **Context Corruption**: Corrupting the `amsiContext` pointer so that initialization fails.
- **Reflection**: Using .NET reflection to set the `amsiInitFailed` flag to `true`, forcing PowerShell to skip scanning.
- **Registry Modification**: Using specific registry keys to disable AMSI for certain providers (e.g., JScript).

### Post-Exploitation Impact
Once AMSI is bypassed within a session, any subsequent script execution (including known malicious payloads like Mimikatz or PowerView) will not be intercepted by the interface.

## Associative Trails
AMSI bypass is a mandatory step in modern Windows post-exploitation when using PowerShell or .NET-based tools.

## Connections
- [[Client-Side Code Execution]]
- [[Antivirus Evasion]]

## Sources
- [[PEN-200-notes.md]] (PEN-300 Section)
