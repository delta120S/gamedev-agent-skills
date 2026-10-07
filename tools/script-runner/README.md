# script-runner — Stable Script Execution Utility
**Purpose:** Stage scripts on stable drive path, Test-Path -LiteralPath before exec, PowerShell case-insensitive variable guard
**Source:** Tools/mcp_on_demand.ps1, PROJECT_MEMORY.md L-103, L-140
**Provenance:** PROJECT_MEMORY.md L-103 (TEMP staging vanished), L-140 (PS case-insensitive vars)
**Tested:** Yes (2026-09-30 L-103, 2026-10-01 L-140)
**Sandbox-Safe:** Yes (no external deps)

## Usage
```powershell
# Stage script to stable path
$stableRoot = "D:\game\gameadv\_SA_TOOLING\staged-scripts"
$scriptPath = Join-Path $stableRoot "myscript.ps1"
if (-not (Test-Path -LiteralPath $scriptPath)) { throw "Script not staged" }
& $scriptPath -Args "value"

# Case-insensitive variable guard
# NEVER: $R = $rlines.Count  (overwrites $r)
# ALWAYS: $lineCount = $rlines.Count
```

## Exit Codes
- 0: Success
- 1: Script not found at staged path
- 2: Test-Path failed (drive/path issue)
- 3: Script execution error

## Key Lessons
- **L-103:** `%TEMP%\opencode` staging vanished between runs → stage on stable drive (`D:\game\gameadv\_SA_TOOLING\staged-scripts`)
- **L-140:** PS variable names case-insensitive → `$R` overwrites `$r`; never reuse var with only case difference
- **L-140:** Scope insert searches to header block (e.g., `-lt 200` lines) not whole file
- **L-140:** Keep `ErrorActionPreference = Stop` so bad assumptions fail before mutating