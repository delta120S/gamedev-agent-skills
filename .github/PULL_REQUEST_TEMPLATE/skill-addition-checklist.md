---
name: Skill Addition Checklist
description: Checklist for adding/updating skills, tools, or prompt patterns
title: "[SKILL] <skill-id>: <title>"
labels: ["skill-addition"]
body:
  - type: markdown
    attributes:
      value: |
        ## Skill Addition PR Checklist
        Complete all items before requesting review.
  - type: checkboxes
    id: skill-checks
    attributes:
      label: Skill Quality (Appendix E)
      options:
        - label: E1 Triggers — ≥3 positive + ≥2 not-for phrases, unambiguous
        - label: E2 Procedure — Reproducible by naive agent (no hidden context, placeholders defined)
        - label: E3 Provenance — ≥90% steps tagged; rest UNVERIFIED-marked
        - label: E4 Pitfalls — ≥1 real past failure with symptom→cause→fix
        - label: E5 Verify — Executable with expected evidence + fail-action
        - label: E6 Escalate — Defines stop + handoff IDs
        - label: E7 Size — Within [C-06]; depth in references/
        - label: E8 Secrets — Zero secrets/PII/absolute personal paths (placeholders only)
        - label: E9 Cross-links — Resolve; no body duplication
        - label: E10 Frontmatter — Complete per B2; draft flag honest
        - label: E11 Scripts — Header complete + sandbox-safe flag + tested status (if any)
        - label: E12 Critic Pass — Notes exist under passes/; revisions applied or justified
  - type: checkboxes
    id: tool-checks
    attributes:
      label: Tool Quality (if adding tool)
      options:
        - label: README.md complete (Purpose, Usage, Exit Codes, Sandbox-Safe, Tested, Provenance, Key Lessons)
        - label: No external dependencies beyond Unity Editor API or standard CLI
        - label: Exit codes documented (0=success, 1+=specific failures)
  - type: checkboxes
    id: prompt-checks
    attributes:
      label: Prompt Pattern Quality (if adding prompt)
      options:
        - label: Template with placeholders (no absolute paths/secrets)
        - label: When to Use / When NOT to Use clearly defined
        - label: ANTI-LOOP section included
        - label: Skills required listed
        - label: Provenance traced to journey artifacts
  - type: checkboxes
    id: repo-checks
    attributes:
      label: Repository Standards
      options:
        - label: Follows Appendix G structure
        - label: catalog/INDEX.md updated (bijection with skills/ folders)
        - label: LICENSE consistent (repo + frontmatter)
        - label: CHANGELOG.md updated with new entries
        - label: Secret scan clean (Appendix F)
  - type: textarea
    id: critic-link
    attributes:
      label: Critic Pass Notes Link
      description: Link to passes/<skill-id>/critic_pass.md or N/A
      placeholder: passes/unity-new-skill/critic_pass.md
    validations:
      required: true
  - type: textarea
    id: design-note
    attributes:
      label: Design Note Link
      description: Link to design-notes/DESIGN_NOTE-<phase>.md for key decisions
      placeholder: design-notes/DESIGN_NOTE-4.md