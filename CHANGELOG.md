# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-10-07

### Added — Skills (24)

#### Compile & Repo Forensics (Batch 1)
- `unity-compile-triage` — Unity Compile Error Triage & First Response (verified)
- `unity-duplicate-tree-surgery` — Unity Duplicate Asset/Object Surgery (verified)
- `unity-console-forensics` — Unity Console Baseline & Error Family Classification (verified)
- `unity-texture-compression-migration` — Texture Compression Migration PVRTC→ASTC (verified)
- `references/serialized-upgrade.md` — ForceReserializeAssets protocol (depth)
- `references/urp-renderer-restore.md` — Git-last-good URP restore (depth)

#### Environment Pipeline (Batch 2)
- `blender-unity-terrain-pipeline` — Blender→Unity Terrain Pipeline (verified)
- `marrakesh-terrain-texturing` — Marrakesh Terrain Texturing (verified)
- `mountain-ring-sculpt` — Atlas Mountain Ring Sculpt (verified)
- `hdri-sky-system` — HDRI Sky System 2-3 States (verified)
- `fog-horizon-matching` — Fog & Horizon Matching Exp2 (verified)

#### Runtime Systems (Batch 3)
- `traffic-waypoint-generation` — Traffic Waypoint Generation (verified)
- `car-ai-lane-discipline` — Car AI Lane Discipline (verified)
- `prop-colliders-pass` — Prop Collider Audit & Assignment (verified)
- `mobile-quality-tiers` — Mobile Quality Tiers Auto-Detect (verified)
- `perf-pass-protocol` — Perf Pass Protocol (verified)

#### Meta & Ops (Batch 4)
- `qa-capture-protocol` — QA Capture Protocol (verified)
- `quarantine-protocol` — Quarantine Protocol (verified)
- `git-hygiene-for-unity` — Git Hygiene for Unity (verified)
- `guid-meta-surgery` — GUID + Meta Surgery (verified)
- `ui-listeners-hygiene` — UI Listener Hygiene (verified)
- `prompt-engineering-gamedev` — Prompt Engineering for Game Dev (verified)
- `overnight-autonomy-ops` — Overnight Autonomy Ops (verified)
- `mcp-unity-operations` — MCP Unity Operations (verified)
- `skill-distillation` — Skill Distillation Meta-Skill (verified)

#### Specialized
- `blender-game-animation` — Blender Game Animation Rig/Skin/Animate/Export (verified)
- `fal-ai-generation` — fal.ai Generation API + Model Rotation (verified)
- `character-sheet-pipeline` — Character Sheet Pipeline (mixed)

### Added — Tools (10)
- `mcp-manager` — MCP Server Toggle Utility (tested: 2026-10-05)
- `script-runner` — Stable Script Execution Utility (tested: 2026-09-30)
- `blender-export` — Unity→Blender Mesh Export (tested: 2026-10-01)
- `perf-ladder` — Depth Ladder Performance Measurement (tested: 2026-10-01)
- `guid-swap` — GUID + Meta Surgery Utility (tested: 2026-09-24)
- `console-parser` — Console Baseline Export (tested: 2026-10-07)
- `ref-scan` — AssetDatabase Reverse Dependency Scanner (tested: 2026-10-06)
- `dup-hash-scanner` — Content-Hash Duplicate Asset Scanner (tested: 2026-10-06)
- `quarantine-indexer` — QUARANTINE.md Ledger Manager (tested: 2026-10-06)

### Added — Prompt Patterns (6)
- `forensic-cleanup` — Duplicate assets, dead files, orphan refs
- `environment-rescue` — Sky/sun/fog/horizon, terrain extent, mountain sculpt
- `traffic-generation` — Waypoint graph, median incidents, lane discipline
- `overnight-autonomy` — Unattended multi-phase with state survival
- `runtime-fix-sweep` — Console errors, listener mismatches, perf regression
- `skill-distillation` — This pipeline as self-hosting meta-skill

### Infrastructure
- MIT License
- Appendix N catalog index (catalog/INDEX.md)
- Architecture docs (docs/ARCHITECTURE.md)
- Maintenance guide (docs/MAINTENANCE.md)
- GitHub issue/PR templates + skill-lint workflow