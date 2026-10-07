---
name: Skill Request
description: Request a new skill or improvement to existing skill
title: "[SKILL] <skill-id>: <title>"
labels: ["skill-request"]
body:
  - type: markdown
    attributes:
      value: |
        ## Skill Request
        Use this template to request a new skill or significant improvement to an existing skill.
  - type: input
    id: skill-id
    attributes:
      label: Skill ID
      description: kebab-case domain-capability (e.g., unity-duplicate-tree-surgery)
      placeholder: unity-new-capability
    validations:
      required: true
  - type: input
    id: title
    attributes:
      label: Title
      description: Human-readable skill name
      placeholder: Unity New Capability Handler
    validations:
      required: true
  - type: textarea
    id: triggers
    attributes:
      label: Trigger Phrases
      description: ≥3 positive trigger phrases and ≥2 not-for phrases
      placeholder: |
        When: "trigger phrase 1", "trigger phrase 2", "trigger phrase 3"
        Not for: "not-for phrase 1", "not-for phrase 2"
    validations:
      required: true
  - type: textarea
    id: provenance
    attributes:
      label: Provenance Sources
      description: Source artifacts + extract refs (e.g., PROJECT_MEMORY.md L-123, prompt_dump.txt F-004)
      placeholder: |
        - PROJECT_MEMORY.md §4 L-200
        - prompt_dump.txt F-015
        - extracts/new_source.md F-001..F-010
    validations:
      required: true
  - type: dropdown
    id: confidence
    attributes:
      label: Confidence Flag
      description: Evidence depth
      options:
        - verified (≥90% provenance, real failures)
        - mixed (single source or partial evidence)
        - inferred (>30% UNVERIFIED, draft: true)
    validations:
      required: true
  - type: textarea
    id: pitfall
    attributes:
      label: Real Past Failure (Required)
      description: At least one real failure from journey with symptom → cause → fix
      placeholder: |
        Symptom: <what happened>
        Cause: <root cause>
        Fix: <what fixed it>
    validations:
      required: true
  - type: checkboxes
    id: checklist
    attributes:
      label: Pre-submission Checklist
      options:
        - label: Two-Pass Law followed (author → cooling → critic)
        - label: Appendix E checklist all 12 checks pass
        - label: Critic pass notes saved under passes/<skill-id>/
        - label: Frontmatter complete per SPEC.md
        - label: Zero secrets/PII/absolute paths
        - label: Cross-links resolve (sibling skills by ID only)
        - label: Size 40-400 lines in SKILL.md (depth in references/)
        - label: DESIGN_NOTE-<phase> written for key decisions