# Prompt Pattern: overnight-autonomy
**When to Use:** Unattended multi-phase missions with state survival, budget stops, phone-lab batching
**When NOT to Use:** Interactive sessions, single-phase fixes, missions requiring human decisions

## Template
```markdown
# OPERATION OVERNIGHT-AUTONOMY — <project> | autonomous | state survival

## 0. ROLE & AUTONOMY
[0-01] Principal engineer + automation architect; unattended 8+ hours; state survival mandatory.
[0-02] MCP on-demand presets (full/unity/device/min); stable script staging; Test-Path before exec.
[0-03] Budget stops: context ~70% → dump state+ledger; phase overrun → DEFERRED + commit + continue.

## 1. MISSION & SUCCESS
[1-01] Execute multi-phase mission spec with zero human intervention.
[1-02] State survival: ledger + state dumped at context guard; resume from extracts.
[1-03] Budget enforcement: phase time budgets; overrun → park batches DEFERRED.
[1-04] Phone-lab batching: device test batches queued; results aggregated.
[1-05] SUCCESS = all phases complete or DEFERRED with evidence; state intact; ledger current.

## 2. CONSTITUTION
[C-01] READ-ONLY SOURCES for analysis phases; extraction copies only.
[C-02] SECRET-FREE: no credentials of any kind in any generated config.
[C-03] GIT LAW: fresh repo for outputs; commit per gate; tags per phase.
[C-04] APPEND-ONLY: DECISIONS-LOG, ledger, state, CONTRADICTIONS.
[C-05] TOOLING GUARD: local install = copies; never overwrite user configs.

## PHASE 0 — MCP & ENV SETUP (15m)
[P0-01] mcp-manager: preset unity (unity-mcp + remotes only); verify 1 mcp-for-unity.exe.
[P0-02] script-runner: stage all phase scripts to stable path; Test-Path -LiteralPath each.
[P0-03] State dump: write state.md + ledger.md to extracts/ for resume.

## PHASE 1-N — MISSION PHASES (per spec)
[P1-01] Execute phase per mission spec; append cycle line to ledger.
[P1-02] Context check: if ~70% → dump state+ledger; continue from extracts.
[P1-03] Budget check: if overrun → park remaining DEFERRED + commit + continue.
[P1-04] Gate verification: Appendix E checks per skill; commit + tag.

## PHASE N+1 — DEVICE TEST BATCHING (if applicable)
[P2-01] android/maestro MCP: list devices; queue test batch.
[P2-02] Execute batch; aggregate perf.csv, markers.csv, toggles.csv.
[P2-03] Slice tails by file order (decreasing t); stamp with segment+build.
[P2-04] Battery ≥60% precondition; svc power stayon true; KEYCODE_WAKEUP before install.

## PHASE N+2 — HANDOVER (15m)
[P3-01] Final report: per-defect symptom|root cause|fix|evidence|shots.
[P3-02] Lessons appended (next L-number).
[P3-03] State = COMPLETE; final commit + tag.

## ANTI-LOOP
[A-01] Retry ≤3 with change; identical command never 4th; same output twice = switch strategy.
[A-02] Author cycles ≤2 revisions/skill → DRAFT-flag and move on.
[A-03] SCAN-ONCE; gated never re-run; blocked 3× → BLOCKED doc.
[A-04] Context ~70% → dump state+ledger; continue from extracts.
[A-05] Self-catch → STOP, read state, jump to next OPEN ledger item.

## SKILLS REQUIRED
- overnight-autonomy-ops (primary)
- mcp-unity-operations
- script-runner tool
- perf-ladder tool (device measurements)
- All mission-specific skills

## PLACEHOLDERS
- <project>: Unity project name
- <mission-spec>: path to mission spec markdown
- <phase-budgets>: time budgets per phase
- <device-serial>: target device serial (if device phase)