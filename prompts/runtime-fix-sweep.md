# Prompt Pattern: runtime-fix-sweep
**When to Use:** Console errors, listener mismatches, RESTYLE spam, perf regression, NRE/MRE spikes
**When NOT to Use:** Structural duplicate fixes, environment/sky/terrain fixes, traffic/graph fixes, build pipeline issues

## Template
```markdown
# OPERATION RUNTIME-FIX-SWEEP — <project> | autonomous

## 0. ROLE & AUTONOMY
[0-01] Senior Unity runtime engineer; unattended; finish or BLOCKED-final.
[0-02] Evidence-only: console baseline export; error family classification; listener count; perf ladder.
[0-03] Self-healing watchdogs over one-shot init for anything play-critical.

## 1. MISSION & SUCCESS
[1-01] Console errors → 0 (classify families; duplicate-tree → handoff; else fix top families).
[1-02] Listener count → expected (48 measured); register-once + remove-on-disable.
[1-03] RESTYLE spam → 0 in 60s play; idempotent restyle (boot + theme only).
[1-04] Perf regression → measure→ordered cheap wins→stop-at-target+pair shots.
[1-05] NRE/MRE spikes → self-healing guards (InputReady, BSystemUI, NetworkAnimator).
[1-06] SUCCESS = G0-1 gate: errors 0; listeners == expected; restyle silent; perf target met.

## 2. CONSTITUTION
[C-01] MINIMAL-DIFF: smallest fix per defect; relink canonical first.
[C-02] BATCH law: fixes ≤50 per batch; compile+scan between; commit each.
[C-03] EVIDENCE law: console baseline + error family classification per fix.
[C-04] MID-PLAY COMPILE GUARD: never compile during play unless budgeted.
[C-05] SELF-HEALING: watchdogs/guards for play-critical statics.

## PHASE 0 — CONSOLE TRUTH (20m)
[P0-01] Export console → console-baseline-PS2.txt (counts + families).
[P0-02] Classify: duplicate-tree (CS0101/CS0111...) → STOP, handoff unity-duplicate-tree-surgery.
[P0-03] Else fix top families minimally; target errors == 0.
[P0-04] Listener dump: IDs → locate duplicate → register-once + remove-on-disable → assert at boot.
[P0-05] RESTYLE: idempotent (boot + theme); verify 60s play silence.
[G0-1] GATE: errors 0; listeners == expected; restyle silent; tag ps2-pre-fixes.

## PHASE 1 — PERF PASS (25m)
[P1-01] FPS on route (editor play, scale 1.0, tier Medium): min/avg.
[P1-02] If avg <45: cheap wins IN ORDER, re-measure each:
  shadow distance per tier → LOD bias 0.25 → terrain LOD distances → texture memory → static occlusion.
[P1-03] Stop at first win reaching target; table recorded.
[P1-04] Pair shots before/after wins judged (no visual regression).
[G1-1] GATE: FPS table; target met or wins exhausted+documented.

## PHASE 2 — NRE/MRE GUARDS (20m)
[P2-01] Audit: InputReady, BSystemUI.Instance, NetworkAnimator cache — all have watchdogs?
[P2-02] Add RuntimeInitializeOnLoadMethod self-heal guards where missing.
[P2-03] Verify: mid-play compile → play test → 0 NRE on critical systems.
[G2-1] GATE: guards verified; commit.

## ANTI-LOOP
[A-01] Retry ≤3 with change; identical command never 4th; same output twice = switch strategy.
[A-02] Fix ≤3/defect; verify ≤4/phase; two zero-delta = LOOP.
[A-03] SCAN-ONCE; blocked 3× → BLOCKED doc.

## SKILLS REQUIRED
- unity-console-forensics (primary)
- unity-compile-triage (duplicate-tree handoff)
- perf-pass-protocol
- git-hygiene-for-unity

## PLACEHOLDERS
- <project>: Unity project name
- <expected-listeners>: measured constant (48)
- <perf-target>: FPS target (45 editor / 30 device)