# GameDev Agent Skills Catalog
**Generated:** 2026-10-07 | **Version:** 1.0.0 | **Skills:** 4 implemented (20 planned) | **Tools:** 10 | **Prompts:** 6

## Philosophy
This catalog indexes a provenance-first, granularity-law skill library for Unity game development. Every skill traces its procedures to source artifacts via `[src:<extract>#F-<n>]` tags. Confidence flags indicate evidence depth: **verified** (≥90% provenance, real failures), **mixed** (single source), **inferred** (>30% UNVERIFIED, published with `draft: true`).

---

## Implemented Skills (Batch 1: Compile & Repo Forensics)

| Skill ID | Title | Triggers (Compressed) | Scripts | Refs | Confidence | Prov. Count |
|---|---|---|---|---|---|---|
| `unity-compile-triage` | Unity Compile Error Triage | CS errors, duplicate types, NRE/MRE, plugin conflicts | — | 3 | verified | 6 |
| `unity-duplicate-tree-surgery` | Unity Duplicate Asset Surgery | duplicate textures/materials/prefabs, spaced names, zero-ref | — | 2 | verified | 5 |
| `unity-console-forensics` | Unity Console Baseline & Classification | console baseline, error families, listener count, RESTYLE | — | 3 | verified | 4 |
| `unity-texture-compression-migration` | Texture Compression PVRTC→ASTC | PVRTC, ASTC 6x6, sRGB discipline, reimport storm | — | 2 | verified | 4 |

## Planned Skills (Per Taxonomy — Not Yet Authored)

### Batch 1 Remaining (Compile & Repo Forensics)
| Skill ID | Title | Trigger Family | Confidence | Notes |
|---|---|---|---|---|
| unity-serialized-upgrade | Unity Serialized Asset Upgrade | ForceReserializeAssets batching | mixed | Demoted to references/ in unity-compile-triage |
| urp-renderer-features-restore | URP Renderer Feature Restore | git-last-good restore-first | mixed | Demoted to references/ in unity-compile-triage |

### Batch 2: Environment Pipeline
| Skill ID | Title | Trigger Family | Confidence | Notes |
|---|---|---|---|---|
| blender-unity-terrain-pipeline | Blender→Unity Terrain Pipeline | terrain extent, mountain sculpt | verified | 5 atoms |
| marrakesh-terrain-texturing | Marrakesh Terrain Texturing | triplanar PBR, dual slope/altitude | verified | 2 atoms |
| mountain-ring-sculpt | Atlas Mountain Ring Sculpt | ridge multifractal, wadi gullies | verified | 3 atoms |
| hdri-sky-system | HDRI Sky System | GTASkybox, Trilight ambient | verified | 2 atoms |
| fog-horizon-matching | Fog & Horizon Matching | weather-type authority, exp2 tints | verified | 2 atoms |

### Batch 3: Runtime Systems
| Skill ID | Title | Trigger Family | Confidence | Notes |
|---|---|---|---|---|
| traffic-waypoint-generation | Traffic Waypoint Generation | 3008 nodes, 93 roads | verified | 3 atoms |
| car-ai-lane-discipline | Car AI Lane Discipline | stuck/flip, lateral clamp | verified | 2 atoms |
| vehicle-enter-exit-determinism | Vehicle Enter/Exit Determinism | avel vs Speed gate | mixed | Demoted to references/ |
| shader-warmup-boot | Shader Warmup at Boot | VidWarmup.shadervariants | mixed | Demoted to references/ |
| prop-colliders-pass | Prop Collider Audit | 1748 props, probe route | verified | 2 atoms |
| mobile-quality-tiers | Mobile Quality Tiers | RAM tiers, renderScale 0.7/0.85/1.0 | verified | 5 atoms |
| perf-pass-protocol | Perf Pass Protocol | ladder methodology, reset=control | verified | 3 atoms |

### Batch 4: Meta & Ops
| Skill ID | Title | Trigger Family | Confidence | Notes |
|---|---|---|---|---|
| qa-capture-protocol | QA Capture Protocol | MT/CL/ST/TR/TM/HD/VD/PF catalog | verified | 1 atom |
| quarantine-protocol | Quarantine Protocol | reversible deletes, ledger | verified | 2 atoms |
| git-hygiene-for-unity | Git Hygiene for Unity | tags per phase, pair-move | verified | 5 atoms |
| guid-meta-surgery | GUID + Meta Surgery | inbound-refs, scripted swaps | verified | 2 atoms |
| ui-listeners-hygiene | UI Listener Hygiene | 48≠47, register-once | verified | 1 atom |
| minimap-render-fix | Minimap Render Fix | multi-cam union, yaw-sweep | mixed | 1 atom |
| prompt-engineering-gamedev | Prompt Engineering | constitution/phases/gates | verified | 1 atom |
| overnight-autonomy-ops | Overnight Autonomy Ops | MCP presets, budget stops | verified | 2 atoms |
| mcp-unity-operations | MCP Unity Operations | single-client stdio 6401 | verified | 2 atoms |
| antigravity-autonomy-setup | Antigravity Autonomy Setup | auto-run, rules file | mixed | Demoted to references/ |
| github-publish-pipeline | GitHub Publish Pipeline | auth fallbacks, clone-verify | mixed | Demoted to references/ |
| skill-distillation | Skill Distillation Meta-Skill | atoms→skills pipeline | verified | 1 atom |

### Specialized Skills
| Skill ID | Title | Trigger Family | Confidence | Notes |
|---|---|---|---|---|
| blender-game-animation | Blender Game Animation | turns, quadrupeds, rig, weights | verified | 14 atoms |
| fal-ai-generation | fal.ai Generation | nano-banana-2/pro, Grok Imagine 2 | verified | 10 atoms |
| character-sheet-pipeline | Character Sheet Pipeline | single image → 3D reference pack | mixed | 3 atoms |

---

## Tools Index (All Implemented)

| Tool | Purpose | Tested | Provenance |
|---|---|---|---|
| mcp-manager | Toggle MCP servers, kill by exe-stem, opencode reload | 2026-10-05 | L-245 |
| script-runner | Stable drive staging, Test-Path -LiteralPath, PS case guard | 2026-09-30 | L-103, L-140 |
| blender-export | Unique meshes + instance/transform table (line-based) | 2026-10-01 | L-139 |
| perf-ladder | Depth ladder reset=kill=, same-config diffs, context verify | 2026-10-01 | L-130, L-131, L-133 |
| guid-swap | Inbound-refs counting + scripted GUID swaps + proof | 2026-09-24 | L-68, L-112, L-113 |
| console-parser | Console baseline export + error family classification | 2026-10-07 | prompt_dump |
| ref-scan | AssetDatabase reverse deps for canonical selection | 2026-10-06 | DUPLICATES.md |
| dup-hash-scanner | Content-hash duplicate groups + material signatures | 2026-10-06 | DUPLICATES.md |
| quarantine-indexer | QUARANTINE.md ledger + reversible deletes + prune | 2026-10-06 | QUARANTINE.md |

---

## Prompt Patterns Index (All Implemented)

| Family | When to Use | Skills Required |
|---|---|---|
| forensic-cleanup | Duplicate assets, dead files, orphan refs, spaced names | unity-duplicate-tree-surgery, unity-compile-triage, git-hygiene-for-unity, quarantine-protocol |
| environment-rescue | Sky/sun/fog/horizon, terrain extent, mountain sculpt | blender-unity-terrain-pipeline, marrakesh-terrain-texturing, mountain-ring-sculpt, hdri-sky-system, fog-horizon-matching |
| traffic-generation | Waypoint graph corruption, median incidents, lane discipline | traffic-waypoint-generation, car-ai-lane-discipline, qa-capture-protocol |
| overnight-autonomy | Unattended multi-phase with state survival | overnight-autonomy-ops, mcp-unity-operations, perf-ladder tool |
| runtime-fix-sweep | Console errors, listener mismatches, RESTYLE spam, perf | unity-console-forensics, unity-compile-triage, perf-pass-protocol |
| skill-distillation | Distill project journey into reusable agent skills | skill-distillation (self) + all 24 skills + 10 tools + 6 prompts |

---

## Draft Register
*None — all 4 implemented skills published with `draft: false` and `confidence: verified`*

---

## Maintenance
New lessons → atoms → skills via skill-distillation skill. See [docs/MAINTENANCE.md](docs/MAINTENANCE.md).