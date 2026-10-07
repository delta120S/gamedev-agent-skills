---
name: Unity Compile Error Triage & First Response
description: |
  When Unity Console shows CS-family compiler errors (CS0101, CS0111, CS0229, CS0462, CS8646) OR duplicate-type errors across Assets/ folders OR MissingReferenceException/NullReferenceException at runtime
  Trigger phrases: "compile errors", "CS0101", "CS0111", "CS0229", "duplicate type", "Multiple plugins", "NullReferenceException", "MissingReferenceException"
  Not for: runtime logic bugs without compiler errors, shader compilation errors, package manager resolution failures, IL2CPP link errors
license: MIT
provenance: 6 sources
confidence: verified
created: 2026-10-07
maintainer: agent-skills
draft: false
---

## WHEN
- Unity Console shows compiler errors in CS-family (CS0101, CS0111, CS0229, CS0462, CS8646) — typically duplicate type definitions across `Assets/Gameplay` vs `Assets/The Game` or similar parallel folder structures
- Error "Multiple plugins 'sqlite3'" or similar native plugin conflicts
- Runtime `NullReferenceException` or `MissingReferenceException` spikes after code changes
- Console error count >0 on fresh scene load or play mode entry
- Build fails with Tundra/Bee errors but 0 CS errors (infra-artifact, not code)

## WHY
Compiler errors block all playmode testing and builds. Duplicate-type errors (CS0101/CS0111) are the #1 cause of "works in editor, fails on build" in this project due to the historical `Assets/Gameplay` vs `Assets/The Game` folder split [src:prompt_dump#F-004]. MissingReferenceExceptions often stem from mid-play compiles wiping statics (InputReady, BSystemUI.Instance, NetworkAnimator cache) [src:project_memory#F-018:L-006]. The MINIMAL-DIFF constitution [src:prompt_dump#F-002] demands smallest fix first: relink canonical assets before any code edits.

## PROCEDURE

1. **Export console baseline** → `console-baseline-PS2.txt` with error counts per family [src:prompt_dump#F-004]
2. **Classify error families**:
   - If duplicate-tree family present (CS0101/CS0111/CS0229/CS0462/CS8646 pairs across parallel Assets/ folders) → STOP, hand off to `unity-duplicate-tree-surgery` skill, resume after [src:prompt_dump#F-004]
   - If "Multiple plugins 'sqlite3'" → check `Plugins/` folders for duplicate native libraries, keep one per architecture [src:sadocs_inventory#F-003]
   - Else: proceed to step 3 [src:prompt_dump#F-004]
3. **Fix top error families minimally** — target errors == 0; leftovers → DEFERRED with reason [src:prompt_dump#F-004]
4. **Listener count mismatch (48≠47)**: Dump listener IDs; locate duplicate registration; fix register-once + remove-on-disable; assert count == expected at boot; proof log line [src:prompt_dump#F-004]
5. **RESTYLE spam**: Make restyle idempotent (boot + theme change only); verify absent in 60s play [src:prompt_dump#F-004]
6. **Mid-play compile guard**: Never compile during play unless plan explicitly budgets reload fallout [src:project_memory#F-017:M6]
7. **Self-healing watchdogs**: For anything play-critical (InputReady, BSystemUI, NetworkAnimator), use self-healing watchdogs/guards over one-shot init [src:project_memory#F-018:L-006]
8. **Verify**: Console errors == 0; listeners == expected; restyle silent; commit + tag `ps2-pre-fixes` [src:prompt_dump#F-004]

## PITFALLS

| Symptom | Cause | Fix |
|---|---|---|
| CS0101/CS0111 duplicate types across Assets/Gameplay vs Assets/The Game | Historical folder split; same classes in both trees | Canonical-by-refs: keep most inbound refs, relink others, quarantine duplicates [src:sadocs_duplicates#F-012] |
| "Multiple plugins 'sqlite3'" | Duplicate native plugins in Plugins/ folders | Audit Plugins/ per architecture; keep one; document in QUARANTINE.md [src:sadocs_inventory#F-003] |
| NullReferenceException on InputReady/BSystemUI/NetworkAnimator after mid-play compile | Statics wiped by domain reload | Self-healing watchdogs/guards with RuntimeInitializeOnLoadMethod [src:project_memory#F-018:L-006] |
| 48 listeners registered, 47 expected | Duplicate AddListener without RemoveListener on disable | Register-once pattern + RemoveListener in OnDisable; assert at boot [src:prompt_dump#F-004] |
| RESTYLE spam in console every frame | Restyle called in Update() not idempotent | Idempotent restyle: only on boot + theme change; verify 60s play silence [src:prompt_dump#F-004] |
| Build fails "Can't verify script data layout" but 0 CS errors | Tundra infra-artifact, not code error | Verdict = Tundra success + 0 CS; ignore trailing Failure line [src:project_memory#F-018:L-073] |

## VERIFY

| Check | Command / Method | Expected Evidence | Fail Action |
|---|---|---|---|
| Console errors | Export console → count | Errors: 0 | Re-run classification; fix top family |
| Listener count | Dump listener IDs at boot | Count == expected (48) | Audit duplicate registration |
| RESTYLE spam | Play 60s, grep console | 0 RESTYLE lines | Make idempotent (boot + theme only) |
| Mid-play compile | Verify no compile during play | No domain reload in playmode | Add self-healing watchdogs |
| Tag created | `git tag ps2-pre-fixes` | Tag exists | Create tag |

## ESCALATE

| Stop Condition | Handoff Skill | Human Ask |
|---|---|---|
| Duplicate-tree family detected (CS0101/CS0111 pairs) | `unity-duplicate-tree-surgery` | Never — auto-handoff |
| >3 fix cycles on same error family | `unity-duplicate-tree-surgery` (if structural) or `unity-console-forensics` (if runtime) | Only if all automated fixes exhausted |
| Build fails with 0 CS errors but Tundra errors persist | — | Report infra issue; verify Tundra version |

## REFERENCES

- `references/duplicate-tree-protocol.md` — Canonical-by-refs selection, quarantine, merge-back safety net
- `references/console-families.md` — Full error family taxonomy with regex patterns
- `references/listener-hygiene.md` - Register-once/remove-on-disable patterns + assertion code
- `skills/unity-duplicate-tree-surgery/SKILL.md` - Handoff for structural duplicates
- `skills/unity-console-forensics/SKILL.md` - Handoff for runtime error classification
- `git-hygiene-for-unity` - Tag/commit protocol for fix verification (see skills catalog)