# perf-ladder — Depth Ladder Performance Measurement
**Purpose:** Depth ladder with reset=kill=, same-config diffs, context column verification
**Source:** PROJECT_MEMORY.md L-130, L-131, L-133 (PERF-60 methodology)
**Provenance:** PROJECT_MEMORY.md L-116..L-133
**Tested:** Yes (2026-10-01 PERF-60 mission)
**Sandbox-Safe:** Yes (reads perf.csv, markers.csv, toggles.csv on device)

## Usage
```powershell
# Phase ladder: name:payload format
.\phase_ladder.ps1 -Phase "ship" -Payload "kill=sky,enviro,shadows,rs"
.\phase_ladder.ps1 -Phase "reset" -Payload "kill="
```

## Protocol (L-130, L-131, L-133)
1. **Control window** — baseline with all systems on
2. **Test phases** — each phase = one config file rewritten completely
3. **Context columns** — read back `rs`, `rend`, `cpu`, `gpu` for EVERY phase; assert matches intent (L-131)
4. **Reset phase** — `reset:kill=` at end of ladder; diff against control (L-130) — ladder without reset is VOID
5. **Same-config diffs only** — never add lever savings across configurations (L-133); quote only `(same-config A) - (same-config A minus one lever)`
6. **File-order tail** — slice `perf.csv` by decreasing `t` (file order), not full file analysis (L-123)

## Exit Codes
- 0: Ladder valid (reset == control within 1 vsync tick)
- 1: Reset != control (ladder invalid)
- 2: Context column mismatch (sticky key detected)
- 3: Device telemetry missing/corrupt

## Key Lessons
- **L-120:** `delayCall` NEVER — use `EditorApplication.update` with self-removing delegate
- **L-123:** Delete CSVs while app stopped; slice tails by FILE ORDER
- **L-124:** Split `ExtraKeys` on WHITESPACE only; commas legal inside value
- **L-125:** Absent keys keep current value → re-assert shipped field initialisers in EVERY window
- **L-127:** Never call lever "free" without measure; prove instantiated (GUID ref >=1 outside own .meta)
- **L-129:** Never publish `PlayerLoop - Σ(markers)` — 4 markers missing, overlap breaks median math
- **L-130:** Ladder void unless `reset:kill=` reproduces control
- **L-131:** Sticky key silently changes control group — read back ALL context columns
- **L-133:** Savings NOT additive across configs; non-additivity quantised to vsync tick