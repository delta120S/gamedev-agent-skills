# Contributing to GameDev Agent Skills

Thank you for contributing! This library follows strict provenance and granularity standards.

## Skill Contribution Checklist

Before submitting a skill, ensure:

- [ ] **Provenance**: Every PROCEDURE step has `[src:<extract>#F-<n>]` tag or `UNVERIFIED — inferred` marker
- [ ] **Granularity**: One capability/trigger family; 40-400 lines in SKILL.md; depth in references/
- [ ] **Format**: Frontmatter complete (name, description with ≥3 triggers + ≥2 not-for, license: MIT, provenance, confidence, created, maintainer, draft)
- [ ] **Sections**: WHEN, WHY, PROCEDURE, PITFALLS (≥1 real failure), VERIFY (executable), ESCALATE, REFERENCES
- [ ] **Secrets**: Zero tokens, keys, passwords, personal paths, machine names
- [ ] **Cross-links**: Sibling skills referenced by ID only; no body duplication
- [ ] **Appendix E**: All 12 checks pass (run critic pass)
- [ ] **Critic Pass**: Pass 2 notes saved under `passes/<skill-id>/critic_pass.md`

## Tool Contribution Checklist

- [ ] README.md with: Purpose, Usage, Exit Codes, Sandbox-Safe flag, Tested date, Provenance, Key Lessons
- [ ] No external dependencies beyond Unity Editor API or standard CLI
- [ ] Exit codes documented (0=success, 1+=specific failures)

## Prompt Pattern Contribution Checklist

- [ ] Template with placeholders (no absolute paths/secrets)
- [ ] When to Use / When NOT to Use clearly defined
- [ ] ANTI-LOOP section included
- [ ] Skills required listed
- [ ] Provenance traced to journey artifacts

## Pull Request Process

1. Fork and create feature branch
2. Add skill/tool/prompt following checklists
3. Run `skill-lint.yml` workflow (frontmatter + links + size)
4. Submit PR with:
   - Skill ID and title
   - Provenance summary (source artifacts + extract refs)
   - Confidence flag justification
   - Critic pass notes link
4. Maintainer review → merge → tag

## Code of Conduct

- Be respectful and constructive
- Evidence-based discussions only (cite extract refs)
- No invention — thin evidence → UNVERIFIED marker