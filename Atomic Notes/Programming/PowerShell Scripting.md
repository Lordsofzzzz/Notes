---
status: seedling
tags: [programming, powershell, windows, scripting]
created: 2026-04-19
updated: 2026-04-19
---

# PowerShell Scripting

## Summary
Object-oriented automation and configuration management for Windows environments.

## Details
**Core Logical Content Mandate**:
Unlike Bash, PowerShell passes **objects** rather than just text streams, allowing for direct access to system properties.

### Essentials
- **Help**: `Get-Help <cmdlet> -Examples`.
- **Introspection**: `$var | Get-Member` (view properties and methods).
- **Execution Policy**: `Set-ExecutionPolicy RemoteSigned` or bypass via `powershell -ep bypass`.

### Pipeline and Objects
PowerShell uses a powerful pipeline to filter and format objects.
```powershell
Get-Service | Where-Object {$_.Status -eq "Running"}
Get-Process | Select-Object Name, CPU | Sort-Object CPU -Descending
```

### Variables and Control Flow
- **Variables**: `$name = "Alice"`.
- **Conditionals**: `-eq`, `-ne`, `-gt`, `-lt`, `-match` (regex).
- **Loops**:
    ```powershell
    foreach ($item in $array) { Write-Output $item }
    for ($i=0; $i -lt 5; $i++) { $i }
    ```

### Registry Access
The registry is treated as a drive.
```powershell
cd HKLM:\SOFTWARE
Get-ChildItem
New-ItemProperty -Path . -Name "Key" -Value "Value"
```

## Associative Trails
PowerShell is the "language of choice" for Windows post-exploitation and Active Directory enumeration.

## Connections
- [[Windows Command Line Essentials]]
- [[Active Directory Basics]]

## Sources
- [[PEN-100-notes.md]]
