# Prompt Pattern: skill-distillation
**When to Use:** Distill project journey into reusable agent skills (this pipeline as self-hosting meta-skill)
**When NOT to Use:** Regular feature development, bug fixes, environment/rescue missions

## Template
```markdown
# OPERATION SKILLFORGE - <project> | autonomous | read-only sources | credentials-free publish

## 0. ROLE & AUTONOMY
[0-01] Principal engineer + technical author + knowledge architect; deliberate before every gate.
[0-02] DESIGN_NOTE-<phase>.md before every gate: options, chosen, rejected with reasons.
[0-03] TWO-PASS LAW: every skill = author pass 1 → cooling → critic pass 2 → revision.
[0-04] COOLING RULE: between pass 1 and 2, work on another skill first.
[0-05] HUMILITY LAW: thin evidence → mark UNVERIFIED — inferred; never assert beyond proof.
[0-06] WISDOM RULE: prefer boring, reversible, documented choice; record why.

## 1. MISSION & SUCCESS
[1-01] Transform ALL journey artifacts → curated library of REUSABLE AI-AGENT SKILLS.
[1-02] Publish as clean GitHub repo (structured, documented, MIT, credentials-free, clone-verified).
[1-03] Install locally into Unity project agent skill paths (Claude/OpenCode/Antigravity).
[1-04] SUCCESS = Appendix H publish checklist green + Appendix E green per skill + catalog complete + local install verified.

## PHASE 1 — SOURCE DISCOVERY & INVENTORY (45m)
[P1-01] Probe all sources; record path|kind|items|size|date|readability; unreachable → SOURCE-BLOCKED.
[P1-02] Primary corpus: _SA_DOCS/*, _SA_QUARANTINE/*, _outbox_street_adventure/*, Unity root.
[P1-03] Agent runtime residues + cloud probes (gh/rclone).
[P1-04] Per-source extract-plan: full-read|grep-sample|tail-sample|index-only + reason.
[P1-05] SOURCES_INVENTORY.md table; DESIGN_NOTE-1 (scope + sampling); commit staging decision.
[G1-1] Inventory complete; [G1-2] ≥90% primary readable; [G1-3] extract-plan per source; [G1-4] DESIGN_NOTE-1.

## PHASE 2 — KNOWLEDGE EXTRACTION & TAXONOMY (60m)
[P2-01] Extract per plan → extracts/<slug>.md (Appendix D format: header + facts with provenance).
[P2-02] Extraction rules per kind: reports→decisions+numbers; audits→object|cause|fix; lessons→atoms.
[P2-03] KNOWLEDGE_ATOMS.md: AT-<nnn> | domain | kind | confidence | provenance | target-skill.
[P2-04] Classify into skill domains (Appendix C seed); merge/split per [C-06]/[D-08]; TAXONOMY.md.
[P2-05] Dedup identical atoms; contradictions → CONTRADICTIONS.md with resolution.
[P2-06] Coverage check: every extract fact mapped ≥1 atom; unmapped → NOISE with reason.
[P2-07] Parameters harvest: every magic number → parameter atoms with provenance.
[G2-1] Atoms complete + coverage clean; [G2-2] taxonomy stable; [G2-3] contradictions resolved; [G2-4] DESIGN_NOTE-2.

## PHASE 3 — SKILLS ARCHITECTURE SPEC (30m)
[P3-01] Adopt Agent-Skills format (Appendix B): skills/<id>/SKILL.md + references/ + scripts/ + assets/.
[P3-02] Body order: WHEN | WHY | PROCEDURE | PITFALLS | VERIFY | ESCALATE | REFERENCES.
[P3-03] Progressive disclosure: SKILL.md ≤200 lines; depth → references/; scripts → header+exit codes+sandbox-safe.
[P3-04] Cross-link by skill-id only; bodies never duplicated.
[P3-05] Naming: kebab-case domain-capability.
[P3-06] SPEC.md written; EXEMPLAR skill authored (pass1→cooling→pass2→Appendix E).
[G3-1] SPEC + exemplar Appendix E green; [G3-2] critic notes saved; [G3-3] DESIGN_NOTE-3.

## PHASE 4 — SKILL AUTHORING BATCHES (120m, ≤6 skills/batch)
[P4-01] B1: compile&repo forensics → B2: environment pipeline → B3: runtime systems → B4: meta&ops.
[P4-02] Per batch: DESIGN_NOTE → pass1 → cooling → pass2 → Appendix E → commit.
[P4-03] Scripts promotion: Tools/ snippets → skill scripts or tools/; sanitize paths; sandbox-test.
[P4-04] PARAMETERS LAW: every harvested number in exactly one skill's parameters table.
[P4-05] PITFALLS LAW: every past failure mode as PITFALL in exactly one skill.
[P4-06] DRAFT-FLAG: >30% UNVERIFIED → confidence: inferred + DRAFT banner.
[G4-1] Per batch: all skills Appendix E green or DRAFT-flagged.
[G4-2] Per batch: passes/ contains both passes per skill.
[G4-3] Per batch: provenance ≥90% steps tagged.
[G4-4] Per batch: commit with batch id.
[G4-5] B4-end: taxonomy fully covered.

## PHASE 5 — TOOLS & PROMPT-PATTERNS (30m)
[P5-01] tools/: standalone utilities (guid-swap, console-parser, ref-scan, dup-hash, quarantine-indexer).
[P5-02] prompts/: prompt-patterns library (forensic-cleanup, environment-rescue, traffic-generation, overnight-autonomy, runtime-fix-sweep, skill-distillation).
[P5-03] Catalog indexes both; bodies never inlined.
[G5-1] Tools READMEs complete + TESTED flags honest.
[G5-2] Prompts placeholder-swept (no absolute paths/credentials).
[G5-3] Commit.

## PHASE 6 — REPO ASSEMBLY + LOCAL INSTALL (30m)
[P6-01] New repo at ../_SA_TOOLING/skillforge-repo (Appendix G structure).
[P6-02] README.md: purpose, philosophy, install matrix, catalog pointer, license, maintenance.
[P6-03] catalog/INDEX.md per Appendix N template.
[P6-04] docs/ARCHITECTURE.md + docs/MAINTENANCE.md (skill-distillation pointer).
[P6-05] Local install: copy skills/ → .claude/skills/ AND .opencode/skills/; append AGENTS.md import lines.
[G6-1] Repo structure matches Appendix G; [G6-2] INDEX ↔ folders bijection; [G6-3] local install verified; [G6-4] commit + tag skillforge-assembled.

## PHASE 7 — QA PASS (45m)
[P7-01] Critic re-run Appendix E per skill; failures → revise ≤2 cycles → else DRAFT-flag.
[P7-02] Cross-skill dedup scan: duplicate procedures → shared references/ + links.
[P7-03] Link scan: every cross-link + reference path resolves; catalog ↔ folders bijection.
[P7-04] SECRET/PII SCAN (Appendix F) over whole repo; hits → scrub + log + rescan until zero.
[P7-05] Fresh-eyes test: 3 random skills simulated by naive agent; ambiguities → fix or UNVERIFIED.
[P7-06] Size law [C-06] enforced repo-wide; splits/merges with ledger updates.
[P7-07] License consistency: frontmatter == repo LICENSE; attributions where needed.
[G7-1] QA table all green or flagged; [G7-2] credential scan zero; [G7-3] fresh-eyes closed/flagged; [G7-4] commit + tag skillforge-qa.

## PHASE 8 — GITHUB PUBLISH (25m)
[P8-01] gh auth; fallback git credential; both fail → BLOCKED-publish with manual steps.
[P8-02] Repo name: short, descriptive, available (gamedev-agent-skills or medina-skills); public unless credential scan leaks → private.
[P8-03] Create repo + push main + tag v1.0.0 + topics + release notes.
[P8-04] CLONE-VERIFY: fresh clone → Appendix E sample on 2 skills + link scan + credential scan.
[P8-05] Release v1.0.0 notes: catalog summary, provenance philosophy, install matrix, maintenance pointer, DRAFT list.
[G8-1] Appendix H checklist green; [G8-2] clone-verify clean; [G8-3] release published; [G8-4] DESIGN_NOTE-8.

## PHASE 9 — HANDOVER (20m)
[P9-01] SKILLFORGE_REPORT.md: sources table, atoms by domain, skills catalog, tools/prompts, contradictions, scrub log, publish URL, local paths, DRAFT register.
[P9-02] Lessons log append: "skillforge: <N> skills published at <url>; <M> draft; blocked: <list>".
[P9-03] Maintenance pointer in DOCTRINE/AGENTS: "new lessons → atoms → skills via skill-distillation skill".
[P9-04] Final message per Appendix J; state=COMPLETE; final commit + tag skillforge-done.
[G9-1] Report + lessons + maintenance pointer complete; [G9-2] ledger zero OPEN; [G9-3] final commit + tag.

## ANTI-LOOP (constitution-grade)
[A-01] Retry ≤3 with change; identical never 4th; same output twice = switch.
[A-02] Author cycles ≤2/skill → DRAFT-flag.
[A-03] SCAN-ONCE; gated never re-run; blocked 3× → BLOCKED doc.
[A-04] Context ~70% → dump state+ledger; continue from extracts.
[A-05] Self-catch → STOP, read state, jump next OPEN.
[A-06] Never full-read >2MB; sample+grep; extracts canonical.
[A-07] No skill authored twice: ledger SKILLED blocks re-authoring.

## SKILLS REQUIRED
- skill-distillation (self-hosting meta-skill)
- All 24 skills from taxonomy
- All 10 tools + 6 prompt patterns
- git-hygiene-for-unity (repo assembly)
- github-publish-pipeline (publish)

## PLACEHOLDERS
- <project>: Unity project name
- <repo-name>: gamedev-agent-skills | medina-skills
- <publish-url>: GitHub repo URL
- <local-paths>: .claude/skills/ and .opencode/skills/