# Prompt Pattern: traffic-generation
**When to Use:** Waypoint graph corruption, median incidents, spawn/despawn failures, lane discipline issues
**When NOT to Use:** Vehicle physics tuning, AI behavior beyond lane discipline, multiplayer sync, UI traffic display

## Template
```markdown
# OPERATION TRAFFIC-GENERATION — <project> | autonomous

## 0. ROLE & AUTONOMY
[0-01] Senior Unity AI/traffic engineer; unattended.
[0-02] Evidence-only: 10,000-step random walk, gizmo overlay verification.
[0-03] Existing traffic graph (NestInWaypoints) EXISTS — repair it, do not regenerate unless proven corrupt.

## 1. MISSION & SUCCESS
[1-01] Fix median incidents: node ON median/island → move to lane centers (NestInWaypoints transforms only).
[1-02] Fix AI defects: stuck/flip detector (<2km/h >3s OR |up·worldUp|<0.5 → reset to lane node).
[1-03] Lateral clamp ≤half-lane with gradual correction; median/vegetation brake within lane.
[1-04] Spawn sanity: orientation aligned to lane; no spawn inside colliders.
[1-05] SUCCESS: 120s ≥8 cars observation = 0 median/sidewalk incidents, 0 flipped, 0 stuck >5s.

## 2. CONSTITUTION
[C-01] MINIMAL-DIFF: move nodes only; preserve connectivity.
[C-02] BATCH law: node moves ≤50 per batch; compile+scan between.
[C-03] EVIDENCE law: gizmo captures TR-01..04; random walk log.
[C-04] NO-REGRESSION: traffic graph, sky, materials untouched except clause-ordered.

## PHASE 1 — GRAPH FORENSICS (25m)
[P1-01] Audit: route nodes nearest median incident zone.
[P1-02] If node ON median → GRAPH DEFECT: move to lane centers; log IDs.
[P1-03] Else → AI DEFECT: implement stuck/flip detector + lateral clamp + median brake.
[G1-1] GATE: defect class identified; fixes scoped.

## PHASE 2 — AI BEHAVIOR FIX (35m)
[P2-01] Stuck/flip detector: speed<2km/h>3s under throttle OR |up·worldUp|<0.5 → reset.
[P2-02] Lateral clamp: ≤half-lane, gradual correction.
[P2-03] Median brake: overlap brake within lane; never cross centerline.
[P2-04] Spawn: orientation aligned; no spawn in colliders.
[G2-1] GATE: code compiles; editor play test clean.

## PHASE 3 — OBSERVATION & VERIFICATION (25m)
[P3-01] 120s observation ≥8 cars: TR-01..04 captures + resets count log (target 0).
[P3-02] Gizmo overlay verified: 100% median vegetation clearance, 0 deadlocks, 0 orphan nodes.
[P3-03] 10,000-step multi-origin random walk: 0 stalls.
[G3-1] GATE: observation clean; commit + tag traffic-generation-done.

## ANTI-LOOP
[A-01] Retry ≤3 with change; identical command never 4th; same output twice = switch strategy.
[A-02] ≤3 fix cycles/defect; ≤4 verify/phase.
[A-03] SCAN-ONCE; blocked 3× → BLOCKED doc.

## SKILLS REQUIRED
- traffic-waypoint-generation (primary)
- car-ai-lane-discipline
- git-hygiene-for-unity
- qa-capture-protocol (TR-01..04 catalog)

## PLACEHOLDERS
- <project>: Unity project name
- <median-zone>: identified at P1-01
- <node-ids>: logged at P1-02