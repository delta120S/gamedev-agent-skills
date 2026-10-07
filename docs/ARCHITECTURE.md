# Architecture: Atoms → Skills Pipeline

## Overview
This document describes the knowledge distillation pipeline that transforms raw journey artifacts into a reusable agent-skills library.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        SKILLFORGE PIPELINE                                   │
└─────────────────────────────────────────────────────────────────────────────┘

PHASE 1: SOURCE DISCOVERY
├── Probe all sources (Appendix A kit)
├── Record: path | kind | items | size | date | readability
├── Assign extract-plan per source (full|grep|tail|index)
├── SOURCES_INVENTORY.md
└── DESIGN_NOTE-1 (scope + sampling)

PHASE 2: KNOWLEDGE EXTRACTION
├── SCAN-ONCE per source per plan
├── extracts/<slug>.md (Appendix D: header + facts with provenance)
├── KNOWLEDGE_ATOMS.md: AT-<nnn> | domain | kind | confidence | provenance | target-skill
├── Classify into skill domains (Appendix C seed)
├── TAXONOMY.md: skill-id | title | trigger family | atom count | sources | confidence
├── Dedup + Contradictions → CONTRADICTIONS.md
├── Coverage check: every extract fact mapped ≥1 atom
├── Parameters harvest: every magic number → parameter atoms
└── DESIGN_NOTE-2 (taxonomy choices)

PHASE 3: SKILLS ARCHITECTURE SPEC
├── SPEC.md (format, size laws, provenance rules, draft semantics, maintenance)
├── Exemplar skill: unity-compile-triage
│   ├── Pass 1 (author)
│   ├── Cooling (work on other skills)
│   ├── Pass 2 (critic) → Appendix E checklist
│   └── passes/unity-compile-triage/critic_pass.md
├── References/ for depth (progressive disclosure)
└── DESIGN_NOTE-3 (format deviations)

PHASE 4: SKILL AUTHORING BATCHES
├── Batch 1: Compile & Repo Forensics (6 skills)
├── Batch 2: Environment Pipeline (5 skills)
├── Batch 3: Runtime Systems (5 skills)
├── Batch 4: Meta & Ops (6 skills + 3 specialized)
├── Per skill: DESIGN_NOTE → Pass 1 → Cooling → Pass 2 → Appendix E → Commit
├── Scripts promotion: Tools/ snippets → skill scripts or tools/
├── Parameters Law: every number in exactly one skill's table
├── Pitfalls Law: every past failure in exactly one skill
└── Draft Flag: >30% UNVERIFIED → confidence: inferred + DRAFT banner

PHASE 5: TOOLS & PROMPT PATTERNS
├── tools/: 9 standalone utilities (README + TESTED flags)
├── prompts/: 6 mission templates (placeholders, anti-loop, when/not)
└── Catalog index (bodies never inlined)

PHASE 6: REPO ASSEMBLY + LOCAL INSTALL
├── New repo: ../_SA_TOOLING/skillforge-repo (Appendix G structure)
├── README.md + LICENSE + CONTRIBUTING.md + CHANGELOG.md
├── catalog/INDEX.md (Appendix N template)
├── docs/ARCHITECTURE.md + docs/MAINTENANCE.md
├── Local install: skills/ → .claude/skills/ + .opencode/skills/
└── AGENTS.md/CLAUDE.md append catalog import lines

PHASE 7: QA PASS
├── Critic re-run Appendix E per skill
├── Cross-skill dedup scan
├── Link scan (catalog ↔ folders bijection)
├── Secret/PII scan (Appendix F) → scrub + rescan until zero
├── Fresh-eyes test (3 random skills, naive agent)
├── Size law enforcement
└── License consistency

PHASE 8: GITHUB PUBLISH
├── gh auth + repo create + push + tag v1.0.0 + release notes
├── CLONE-VERIFY: fresh clone → Appendix E sample + link scan + secret scan
└── DESIGN_NOTE-8 (visibility + naming rationale)

PHASE 9: HANDOVER
├── SKILLFORGE_REPORT.md (full summary)
├── Lessons log append
├── Maintenance pointer in DOCTRINE/AGENTS
└── Final commit + tag skillforge-done
```

## Data Flow

```
Sources (read-only)
    │
    ▼
Extracts/ (Appendix D format — canonical re-read source)
    │
    ▼
Knowledge Atoms (AT-<nnn> — single verified fact/procedure units)
    │
    ▼
Taxonomy (skill domains — merge/split per [C-06]/[D-08])
    │
    ▼
Skills/ (Appendix B format — one capability per skill)
    │   ├── SKILL.md (≤200 lines, 7 sections, provenance tags)
    │   ├── references/ (depth — parameter tables, checklists, rationales)
    │   ├── scripts/ (executable helpers — header, exit codes, sandbox-safe)
    │   └── assets/ (rare — secret-free diagrams/text only)
    │
    ▼
Tools/ (standalone utilities — not bound to one skill)
    │
    ▼
Prompts/ (mission templates — placeholders, anti-loop, when/not)
    │
    ▼
Catalog/ (INDEX.md only — links to skills/, tools/, prompts/)
    │
    ▼
GitHub Repo + Local Install
```

## Key Invariants

| Invariant | Enforcement |
|---|---|
| Read-only sources | [C-01] — extraction copies only; zero writes to source artifacts |
| Secret-free | [C-02] — Appendix F scan pre-push; scrub + log + rescan |
| Provenance tags | [C-07] — every PROCEDURE step ends with `[src:<extract>#F-<n>]` |
| No mega-files | [C-08] — catalog indexes only; bodies in skill folders |
| Granularity | [C-06] — >400 lines → split; <40 lines → merge/demote |
| Local install safe | [C-09] — copies only; suffixed folder + log on collision |
| Draft honesty | [P4-06] — >30% UNVERIFIED → confidence: inferred + DRAFT banner |