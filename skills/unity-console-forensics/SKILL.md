---
name: Unity Console Baseline & Error Family Classification
description: |
  When Unity Console shows errors/warnings needing classification, baseline export needed, listener count verification, or RESTYLE spam detection
  Trigger phrases: "console baseline", "console errors", "error families", "listener count", "RESTYLE spam", "48≠47"
  Not for: compile errors (CS-family) — use unity-compile-triage; shader compile errors; package manager logs; IL2CPP output
license: MIT
provenance: 4 sources
confidence: verified
created: 2026-10-07
maintainer: agent-skills
draft: false
---

## WHEN
- Need to export console baseline with error/warning/log counts (console-baseline-PS2.txt)
- Classify console errors into families for targeted fixing
- Verify UI listener registration count matches expected (48 listeners)
- Detect and eliminate RESTYLE spam (idempotent restyle verification)
- Establish clean console state before/after missions

## WHY
Console noise masks real errors. The POLISH-SWEEP v2 mission [src:prompt_dump#F-004] established: baseline export → classify families → if duplicate-tree family present → handoff to duplicate-tree surgery → else fix top families minimally → target errors == 0. Listener mismatch (48≠47) and RESTYLE spam were specific defects (VID4-09, VID4-10) requiring dedicated fixes. Clean console is a gate requirement (G0-1).

## PROCEDURE

1. **Export console baseline** → `console-baseline-PS2.txt` with total, errors, warnings, logs counts [src:prompt_dump#F-004]
2. **Classify error families** using regex patterns [src:references/console-families.md]:
   - Duplicate-tree: CS0101/CS0111/CS0229/CS0462/CS8646 pairs → STOP, handoff to `unity-duplicate-tree-surgery`
   - Plugin conflicts: "Multiple plugins 'sqlite3'" → audit Plugins/ folders
   - Runtime NRE/MRE: NullReferenceException, MissingReferenceException → self-healing watchdogs
   - Listener mismatch: "Listeners 48≠47" → register-once + remove-on-disable
   - RESTYLE spam: RESTYLE logs every frame → idempotent restyle (boot + theme only)
   - Deprecation: CS0618 → fix or pragma disable
3. **Fix top families minimally** — target errors == 0; leftovers → DEFERRED with reason [src:prompt_dump#F-004]
4. **Listener count fix**: Dump listener IDs; locate duplicate registration; implement register-once pattern; assert count == expected at boot [src:prompt_dump#F-004]
5. **RESTYLE fix**: Make restyle idempotent (OnEnable boot + theme change event only); verify 60s play silence [src:prompt_dump#F-004]
6. **Verify gate G0-1**: Errors 0; listeners == expected; restyle silent; commit + tag `ps2-pre-fixes` [src:prompt_dump#F-004]

## PITFALLS

| Symptom | Cause | Fix |
|---|---|---|
| Duplicate-tree family present (CS0101/CS0111 pairs) | Historical Assets/Gameplay vs Assets/The Game split | Handoff to `unity-duplicate-tree-surgery` — structural fix needed first |
| 48 listeners registered, 47 expected | Duplicate AddListener without RemoveListener on disable | Register-once pattern + RemoveListener in OnDisable; assert at boot [src:prompt_dump#F-004] |
| RESTYLE spam every frame | Restyle called in Update() not idempotent | Idempotent restyle: only on boot + theme change; verify 60s play silence [src:prompt_dump#F-004] |
| NullReferenceException on InputReady/BSystemUI after mid-play compile | Statics wiped by domain reload | Self-healing watchdogs/guards with RuntimeInitializeOnLoadMethod [src:project_memory#F-018:L-006] |
| Console baseline shows 0 errors but runtime crashes | Dead log channel: execute_code console output untrustworthy [src:project_memory#F-018:L-148] | Drive verification from RETURN VALUES, not log absence |

## VERIFY

| Check | Command / Method | Expected Evidence | Fail Action |
|---|---|---|---|
| Console baseline | Export console → console-baseline-PS2.txt | Errors: 0, Warnings: 0, Logs: N | Re-classify; fix top family |
| Listener count | Dump listener IDs at boot | Count == 48 (expected) | Audit duplicate registration |
| RESTYLE spam | Play 60s, grep console for RESTYLE | 0 lines | Make idempotent (boot + theme only) |
| Error families | Regex classification | All errors classified | Add missing patterns |
| Tag created | `git tag ps2-pre-fixes` | Tag exists | Create tag |

## ESCALATE

| Stop Condition | Handoff Skill | Human Ask |
|---|---|---|
| Duplicate-tree family detected | `unity-duplicate-tree-surgery` | Never — auto-handoff |
| >3 fix cycles on same runtime error family | `unity-compile-triage` (if compile-related) | Only if all automated fixes exhausted |
| Console baseline export fails | — | Check MCP connection, Unity Editor state |

## REFERENCES

- `references/console-families.md` — Full error family taxonomy with regex patterns
- `references/listener-hygiene.md` — Register-once/remove-on-disable patterns + assertion code
- `references/console-baseline-format.md` — Baseline export format specification
- `skills/unity-compile-triage/SKILL.md` — Handoff for compile errors
- `skills/unity-duplicate-tree-surgery/SKILL.md` — Handoff for duplicate-tree structural fixes