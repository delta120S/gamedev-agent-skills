<p align="center">
  <img src="docs/assets/banner-repo.png" alt="GameDev Agent Skills Library banner" width="100%">
</p>

# GameDev Agent Skills Library

**Provenance-First • Granularity Law • Secret-Free • MIT Licensed**

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Skills](https://img.shields.io/badge/skills-4-brightgreen.svg)](catalog/INDEX.md)
[![Version](https://img.shields.io/badge/version-v1.0.0-blue.svg)](CHANGELOG.md)

A curated library of reusable AI-agent skills for Unity game development, distilled from the complete "Streets Adventure: Medina Motors" journey (200+ lessons, 85 knowledge atoms, 24 skills, 10 tools, 6 prompt patterns).

## About

<img src="docs/assets/emblem-256.png" alt="SkillForge emblem" width="180" align="right">

**GameDev Agent Skills Library** is a provenance-first collection of agent skills for Unity game development — every procedure step traces back to a real project failure or verified win via `[src:...]` tags. Skills are self-contained, MIT-licensed, and drop-in for Claude Code, OpenCode, and other agents.

Ships with 4 production-verified skills, 10 standalone tools, 6 prompt patterns, and a knowledge catalog built for agent consumption.

<br clear="all">

## Philosophy

- **Provenance-First**: Every procedure step traces to a source line via `[src:<extract>#F-<n>]` tags. Thin evidence is marked `UNVERIFIED — inferred`.
- **Granularity Law**: One skill = one capability/trigger family. >400 lines → split. <40 lines → merge or demote to references.
- **Secret-Free**: Zero tokens, keys, passwords, connection strings, personal paths, or machine names in any published file.
- **Reversible**: Skills are self-contained; references/ hold depth; tools/ are standalone utilities.

## Quick Start

### Install Locally (Unity Project)
```bash
# Clone
git clone https://github.com/<org>/gamedev-agent-skills.git
cd gamedev-agent-skills

# For Claude Code
cp -r skills/* <your-unity-project>/.claude/skills/

# For OpenCode
cp -r skills/* <your-unity-project>/.opencode/skills/
# Add to AGENTS.md: import ./gamedev-agent-skills/catalog/INDEX.md

# For Antigravity
# Add workspace rule importing catalog path
```

### Browse Skills
| Category | Skills |
|---|---|
| **Compile & Repo Forensics** | `unity-compile-triage`, `unity-duplicate-tree-surgery`, `unity-console-forensics`, `unity-texture-compression-migration` |
| **Environment Pipeline** | `blender-unity-terrain-pipeline`, `marrakesh-terrain-texturing`, `mountain-ring-sculpt`, `hdri-sky-system`, `fog-horizon-matching` |
| **Runtime Systems** | `traffic-waypoint-generation`, `car-ai-lane-discipline`, `prop-colliders-pass`, `mobile-quality-tiers`, `perf-pass-protocol` |
| **Meta & Ops** | `qa-capture-protocol`, `quarantine-protocol`, `git-hygiene-for-unity`, `guid-meta-surgery`, `ui-listeners-hygiene`, `prompt-engineering-gamedev`, `overnight-autonomy-ops`, `mcp-unity-operations`, `skill-distillation` |

### Tools (Standalone Utilities)
- `mcp-manager` — Toggle MCP servers, kill by exe-stem, opencode reload
- `script-runner` — Stable drive staging, Test-Path -LiteralPath, PS case-insensitive guard
- `blender-export` — Unique meshes + instance/transform table (line-based format)
- `perf-ladder` — Depth ladder with reset=kill=, same-config diffs, context verification
- `guid-swap` — Inbound-refs counting + scripted GUID swaps + post-scan proof
- `console-parser` — Console baseline export + error family classification
- `ref-scan` — AssetDatabase reverse dependency scanner for canonical selection
- `dup-hash-scanner` — Content-hash duplicate asset + material signature groups
- `quarantine-indexer` — QUARANTINE.md ledger manager (reversible deletes, prune policy)

### Prompt Patterns (Mission Templates)
- `forensic-cleanup` — Duplicate assets, dead files, orphan refs, spaced names
- `environment-rescue` — Sky/sun/fog/horizon, terrain extent, mountain sculpt
- `traffic-generation` — Waypoint graph, median incidents, lane discipline
- `overnight-autonomy` — Unattended multi-phase with state survival
- `runtime-fix-sweep` — Console errors, listener mismatches, RESTYLE spam, perf
- `skill-distillation` — This pipeline as self-hosting meta-skill

## Confidence Flags
- **verified** — ≥90% steps provenance-tagged, ≥1 real past failure in PITFALLS
- **mixed** — Single source or partial evidence
- **inferred** — >30% atoms UNVERIFIED (published with `draft: true` + DRAFT banner)

## Gallery

| Hero Poster | Emblem |
|---|---|
| <img src="docs/assets/hero-poster.png" alt="Hero poster" width="560"> | <img src="docs/assets/emblem-logo.png" alt="Project emblem" width="420"> |
| *SkillForge key art — the provenance-first skill library in action.* | *Project emblem — original mark for the GameDev Agent Skills Library.* |

Brand assets: original AI-generated artwork (Gemini) created for this project; no third-party rights.

## License
MIT — see [LICENSE](LICENSE)

## Contributing
See [CONTRIBUTING.md](CONTRIBUTING.md)

## Maintenance
See [docs/MAINTENANCE.md](docs/MAINTENANCE.md) — new lessons → atoms → skills via `skill-distillation` skill.

## Origin
Distilled from the complete "Streets Adventure: Medina Motors" journey (Unity 6, URP, Mirror, Enviro 3) including:
- POLISH-SWEEP v2 (17 video-4 defects resolved)
- DEEP-FIX-SWEEP v2 (9 phases, weapons purge, UI originality, touch reskin)
- PERF-60 (60fps mobile methodology, 14 lessons)
- OPTIM-V2 (ASTC 6x6, 3-tier quality, occlusion culling, CAMVIS)
- MOBILE-GRAYSCREEN rescue (6 iterations, APK build pipeline)
- Full terrain/sky/mountain pipeline (Blender ↔ Unity)
- 200+ lessons (L-001..L-165+) with full provenance