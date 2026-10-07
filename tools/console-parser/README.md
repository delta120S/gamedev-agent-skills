# console-parser — Unity Console Baseline Export & Classification
**Purpose:** Export console baseline, classify error families, verify listener count, detect RESTYLE spam
**Source:** console-baseline-PS2.txt, prompt_dump.txt, PROJECT_MEMORY.md
**Provenance:** prompt_dump.txt (POLISH-SWEEP v2 Phase 0)
**Tested:** Yes (2026-10-07 baseline export)
**Sandbox-Safe:** Yes (Unity Editor API only)

## Usage
```csharp
// Editor script: Tools > Console > Export Baseline
// Output: console-baseline-PS2.txt
// Format:
// Total: <n>
// Errors: <n>
// Warnings: <n>
// Logs: <n>
// Details: [...]
// Errors Breakdown:
// - CS0618: <n>
// - CS0101/CS0111/CS0229: <n>
// - NullReferenceException: <n>
```

## Classification Patterns
```regex
# Duplicate-tree (STOP → handoff)
CS0101|CS0111|CS0229|CS0462|CS8646

# Plugin conflicts
Multiple plugins.*sqlite3

# Runtime exceptions
NullReferenceException|MissingReferenceException

# Listener mismatch
Listeners? \d+≠\d+

# RESTYLE spam
RESTYLE.*every frame|RESTYLE.*spam

# Deprecation
CS0618
```

## Verification
- Errors == 0
- Listeners == expected (48 measured)
- RESTYLE lines == 0 in 60s play
- Tag: `ps2-pre-fixes`

## Exit Codes
- 0: Baseline exported, all families classified
- 1: Console unreadable (editor compiling or disconnected)
- 2: Duplicate-tree family detected (handoff required)
- 3: Listener count mismatch unresolved after audit

## Key Lessons
- **L-148:** `execute_code` console output UNTRUSTWORTHY — drive verification from RETURN VALUES
- **L-148:** Reflection probe (`typeof(T).GetMethod(...)`) proves assembly loaded, not log absence
- Baseline export must be explicit SaveScene; isDirty false after