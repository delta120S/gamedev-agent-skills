# Prompt Pattern: environment-rescue
**When to Use:** Sky/sun/fog/horizon defects, terrain extent deficit, mountain sculpt needed, HDRI system missing, green horizon band
**When NOT to Use:** Gameplay feature additions, package upgrades, runtime logic bugs, UI fixes

## Template
```markdown
# OPERATION ENVIRONMENT-RESCUE — <project> | Blender via MCP | autonomous

## 0. ROLE & AUTONOMY
[0-01] Senior Unity environment artist + tech artist + Blender sculptor; unattended.
[0-02] Evidence-only claims; pixel-verify every capture; GAME view for visuals.
[0-03] Phase 5 (terrain/mountain) preserves plateau height, city contact, material set, collider continuity.

## 1. MISSION & SUCCESS
[1-01] Fix sky/sun: HDRI system (2-3 states), rig matching, controller determinism, placeholder retirement.
[1-02] Fix fog/horizon: exp2 tints per state, pixel-verify no green band.
[1-03] Fix terrain extent: footprint + 25-40% margin OR mountain-ring inner apron; UV/material preservation.
[1-04] Fix mountain sculpt: Atlas foothill character (ridge multifractal, wadi gullies, scree 30-40°); pass corridors at road exits.
[1-05] SUCCESS = HDRI cycle idempotent; fog verified per state; terrain extent deficit=0; mountain sculpt checklist judged.

## 2. CONSTITUTION
[C-01] MINIMAL-DIFF: relink canonical sky assets first; new assets only if truly absent.
[C-02] BATCH law: terrain/sky batches ≤50; compile+scan between; commit each.
[C-03] EVIDENCE law: pixel-verify captures per state; no change without evidence line.
[C-04] SCENE law: explicit SaveScene; isDirty false.
[C-05] QUARANTINE law: old terrain variant quarantined AFTER verification green.
[C-06] NO-REGRESSION: city objects read-only except clause-ordered colliders; sky/materials untouched except clause-ordered.

## PHASE 1 — SKY/HDRI SYSTEM (45m)
[P1-01] Install SkyHDRI (GTASkybox.shader + GTADayNightAtmosphere.cs).
[P1-02] Configure 2-3 states (Day/Night/DayNightCycle); directional sunlight inverted (LookRotation(-lightDir)).
[P1-03] Trilight ambient: sky/equator/red earth ground bounce; ambient probe SH sum ~1.281; 0 black silhouettes.
[P1-04] Verify Day→Night→Day cycle idempotent; captures HD-01..08 judged.
[G1-1] GATE: HDRI cycle clean; commit.

## PHASE 2 — FOG/HORIZON MATCHING (30m)
[P2-01] Weather-type authority proven (Enviro WeatherType carries lighting+fog).
[P2-02] Configure exp2 tints per state: Clear fogDensity 0.00035, fogColorBlend 0.4, startDistance 40.
[P2-03] Pixel-verify no green horizon band (captures FX-01..04).
[P2-04] Shadow cascade cutoff fix: distance shadow fade lerp(shadow, 1.0, saturate((dist-120)/100)).
[G2-1] GATE: fog verified per state; no green band; commit.

## PHASE 3 — TERRAIN EXTENT (Blender, 45m)
[P3-01] MEASURE: map bounds; current terrain footprint; mountain ring radii + peak heights; extent deficit.
[P3-02] EXTEND: footprint + 25-40% margin OR mountain-ring inner apron; PRESERVE plateau Y -0.05±0.05.
[P3-03] UV/MATERIAL: macro UV 0-1 over new extents; detail UV world-scaled 2m; material reconnected unchanged.
[P3-04] EXPORT/IMPORT: terrain FBX → Unity; replace old (quarantine after verify); Mesh Collider refresh.
[G3-1] GATE: extent deficit=0; UV/material preserved; commit.

## PHASE 4 — MOUNTAIN SCULPT (Blender, 60m)
[P4-01] SCULPT: ridge multifractal (octaves 4-5, lacunarity 2.1), blocky ridges, wadi gullies, scree 30-40°.
[P4-02] PRESERVE: pass corridors road+20m, walls 15-25m taper; ring continuity; outer descent below fog.
[P4-03] OPTIMIZE: ≤50k tris ring, ≤60k tris plains; plateau flatness ≤0.05m; single material slot.
[P4-04] EXPORT/IMPORT: ring FBX → Unity; Mesh Collider; Static; layer Ground; LODGroup preserved.
[P4-05] VERIFY: TM-01..08 (4 map-edge, aerial, horizon, city junction, day+sunset).
[G4-1] GATE: sculpt checklist judged; collider+UV+material preserved; commit.

## ANTI-LOOP
[A-01] Retry ≤3 with change; identical command never 4th; same output twice = switch strategy.
[A-02] ≤3 fix cycles/defect; ≤4 verify/phase; two zero-delta = LOOP.
[A-03] SCAN-ONCE; blocked 3× same cause → BLOCKED doc.

## SKILLS REQUIRED
- blender-unity-terrain-pipeline (primary)
- marrakesh-terrain-texturing
- mountain-ring-sculpt
- hdri-sky-system
- fog-horizon-matching
- git-hygiene-for-unity (tags, commits)
- quarantine-protocol (old terrain variant)

## PLACEHOLDERS
- <project>: Unity project name
- <terrain-bounds>: measured at P3-01
- <mountain-params>: octaves 4-5, lacunarity 2.1, etc.