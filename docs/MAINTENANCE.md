# Maintenance Guide

## Overview
This guide explains how future sessions append new atoms and create new skills using the `skill-distillation` meta-skill.

## Adding New Knowledge

### 1. Capture New Lessons
At mission end, append to `PROJECT_MEMORY.md §4`:
```
- L-<next> (YYYY-MM-DD, <source>): <failure> | <root cause> | <rule> | <assertion>
```

### 2. Extract Atoms
Run the `skill-distillation` skill Phase 2 extraction on new sources:
- New reports → decisions + numbers + outcomes
- New audits → object|cause|fix triples
- New lessons → one atom each
- New configs → convention atoms
- New parameters → parameter atoms with provenance

Append to `KNOWLEDGE_ATOMS.md`:
```
AT-<next> | domain | kind | confidence | provenance list | target-skill
```

### 3. Update Taxonomy
- If atoms fit existing skill → add to skill's references/ or extend PROCEDURE
- If atoms form new trigger family → create new skill (Phase 4 batch)
- If atoms contradict existing → resolve in CONTRADICTIONS.md

## Creating a New Skill

### Prerequisites
- ≥3 atoms with same trigger family
- ≥1 real past failure for PITFALLS
- Provenance for ≥90% of steps

### Process (Two-Pass Law)
1. **DESIGN_NOTE**: Document decision (options, chosen, rejected)
2. **Pass 1 (Author)**: Write SKILL.md + references/ + scripts/ per SPEC.md
3. **Cooling**: Work on another skill first (no immediate self-review)
4. **Pass 2 (Critic)**: Appendix E checklist → critic_pass.md
5. **Revise**: Apply critic notes or justify in critic_pass.md
6. **Commit**: `skillforge(P4): add <skill-id> [gate: ok]`

### Skill Template
Copy `skills/unity-compile-triage/` structure:
```
skills/<new-skill-id>/
  SKILL.md           # Frontmatter + 7 sections
  references/        # Depth (parameter tables, checklists)
  scripts/           # Executable helpers (header, exit codes, sandbox-safe)
  assets/            # Rare (secret-free diagrams only)
```

## Updating Existing Skills

### Minor Updates (typo, clarifications)
- Edit SKILL.md directly
- Commit: `skillforge(P7): clarify <skill-id> <section> [gate: ok]`

### Major Updates (new procedure, new pitfall)
- Follow Two-Pass Law (author → cooling → critic)
- Increment version in frontmatter `created` date
- Commit: `skillforge(P7): update <skill-id> <scope> [gate: ok]`

### Adding References
- Add `.md` to `references/`
- Cross-link from SKILL.md REFERENCES section
- No body duplication

## Adding Tools

### Prerequisites
- Standalone utility (not bound to one skill)
- README.md with: Purpose, Usage, Exit Codes, Sandbox-Safe, Tested, Provenance, Key Lessons
- No external dependencies beyond Unity Editor API or standard CLI

### Process
1. Create `tools/<tool-name>/README.md`
2. Commit: `skillforge(P5): add tool <tool-name> [gate: ok]`

## Adding Prompt Patterns

### Prerequisites
- Mission template with placeholders (no absolute paths/secrets)
- When to Use / When NOT to Use clearly defined
- ANTI-LOOP section included
- Skills required listed
- Provenance traced to journey artifacts

### Process
1. Create `prompts/<family-name>.md`
2. Commit: `skillforge(P5): add prompt <family-name> [gate: ok]`

## Local Install Updates

When skills/tools/prompts are added/updated:
```bash
# Re-run local install
cp -r skills/* <unity-project>/.claude/skills/
cp -r skills/* <unity-project>/.opencode/skills/
# AGENTS.md import lines already present (append-only)
```

## Versioning & Releases

- **Patch** (bug fixes, clarifications): increment CHANGELOG, tag `v1.0.x`
- **Minor** (new skills, tools, prompts): increment CHANGELOG, tag `v1.x.0`
- **Major** (breaking format changes): increment CHANGELOG, tag `vx.0.0`

Release notes must include:
- Catalog summary (skills/tools/prompts added/changed)
- Provenance philosophy reminder
- Install matrix update
- Maintenance pointer
- Known DRAFT skills list

## Governance

- **Maintainer**: agent-skills (this pipeline)
- **Reviews**: Appendix E checklist + critic pass notes required
- **Merges**: Only after `skill-lint.yml` passes (frontmatter + links + size)
- **Deprecation**: Mark `draft: true` with reason; never delete (append-only)

## Emergency Procedures

### Secret Leak Detected
1. Immediate: Remove from repo history (BFG Repo-Cleaner or filter-branch)
2. Rotate compromised credentials
3. Appendix F re-scan entire history
4. Document in CONTRADICTIONS.md + SCRUB log
5. Re-publish with clean history

### Skill Contradiction Found
1. Document in CONTRADICTIONS.md with resolution + reason
2. Update affected skills (Two-Pass Law)
3. Commit with `skillforge(P7): resolve contradiction <skill-ids> [gate: ok]`

### Context Overflow in Session
1. Dump state.md + ledger.md
2. Continue from extracts/ only (never re-read sources)
3. Resume at next OPEN ledger item

## Self-Hosting Loop

The `skill-distillation` skill IS the maintenance pipeline:
```
New mission → Lessons (L-<n>) → Atoms (AT-<n>) → Skills (new or updated)
                                    ↑
                                    └── skill-distillation skill
```

Every future session should:
1. Read `PROJECT_MEMORY.md §0 + §4`
2. Print `APPLIED_LESSONS: L-xxx, ...`
3. Execute mission
4. Append new lessons to `PROJECT_MEMORY.md §4`
5. Run `skill-distillation` Phase 2-4 on new sources
6. Publish updates

This creates a self-reinforcing knowledge flywheel.