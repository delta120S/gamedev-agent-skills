# quarantine-indexer — QUARANTINE.md Ledger Manager
**Purpose:** Manage QUARANTINE.md ledger entries, reversible deletes, prune policy
**Source:** _SA_QUARANTINE/QUARANTINE.md, PROJECT_MEMORY.md L-034, L-035
**Provenance:** sa_quarantine F-001 (7 entries), project_memory L-034 (reference graph law), L-035 (ending-preserving writes)
**Tested:** Yes (2026-10-06 quarantine audit)
**Sandbox-Safe:** Yes (file I/O only)

## Usage
```powershell
# Add entry
.\quarantine-indexer.ps1 -Add -Source "P2" -Asset "Assets/_Recovery/0.unity" -Reason "Auto-saved recovery scene" -Location "_SA_QUARANTINE/P2/Assets/_Recovery"

# List entries
.\quarantine-indexer.ps1 -List

# Prune (30 days after verification green)
.\quarantine-indexer.ps1 -Prune -Days 30
```

## QUARANTINE.md Format
| Phase | Asset | Quarantine Location | Reason |
|---|---|---|---|
| P9 | Assets/_Recovery | D:\[game]\gameadv\_SA_QUARANTINE\P9\Assets\_Recovery | Unreferenced auto-saved recovery scenes |
| P10 | ui_staging | D:\[game]\gameadv\_SA_QUARANTINE\P10\ui_staging | Temporary UI work directory |

## Prune Policy
- 30 days after verification green (skill VERIFY passes)
- OR next major Unity version upgrade
- Never prune if any skill references the quarantined asset

## Reversible Delete Protocol (L-034, L-035)
1. Move file + .meta pair to quarantine location (preserves GUID)
2. QUARANTINE.md ledger entry with timestamp, reason, source location
3. `git add` quarantine location + QUARANTINE.md
4. Verify: `git diff --stat` shows only quarantine move
5. Restore: `git checkout -- <original-path>` + `git rm <quarantine-path>`

## Exit Codes
- 0: Entry added/listed/pruned successfully
- 1: QUARANTINE.md not found
- 2: Asset not found at source
- 3: Prune conditions not met