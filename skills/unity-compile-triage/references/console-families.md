# Console Error Family Taxonomy
**Referenced from:** `skills/unity-compile-triage/SKILL.md` step 2, REFERENCES
**Source:** prompt_dump.txt, PROJECT_MEMORY.md, console-baseline-PS2.txt

## Error Families (Priority Order)

### 1. Duplicate-Tree Family (HIGHEST PRIORITY — STOP & HANDOFF)
- **CS0101** — Type already defined (same class/struct in multiple assemblies)
- **CS0111** — Type already defines a member with same parameter types
- **CS0229** — Ambiguity between inherited members
- **CS0462** — Base class member hides inherited member
- **CS8646** — Nullability mismatch in overrides
- **Pattern:** Pairs across `Assets/Gameplay` vs `Assets/The Game` or similar parallel folders
- **Action:** STOP → handoff to `unity-duplicate-tree-surgery`

### 2. Native Plugin Conflicts
- **Error:** "Multiple plugins 'sqlite3'" (or similar)
- **Cause:** Duplicate .dll/.so/.bundle in Plugins/ folders per architecture
- **Action:** Audit Plugins/ per architecture (x86, x64, ARM64); keep one; QUARANTINE.md

### 3. MissingReferenceException (Runtime)
- **Cause:** Destroyed object still referenced; mid-play compile wiped statics
- **Common targets:** InputReady, BSystemUI.Instance, NetworkAnimator cache
- **Fix:** Self-healing watchdogs/guards over one-shot init [src:project_memory#F-018:L-006]

### 4. NullReferenceException (Runtime)
- **Cause:** Uninitialized field; missing component; destroyed GameObject
- **Common:** PlayerHealth, UI managers, NetworkIdentity
- **Fix:** Guards + null-checks; verify prefab overrides == code values [src:project_memory#F-008]

### 5. CS0618 Deprecation Warnings
- **Target:** 0 remaining per [P0-01]
- **Action:** Fix or `#pragma warning disable` with comment

### 6. Listener Registration Mismatch
- **Symptom:** "Listeners 48≠47" or similar count mismatch
- **Cause:** Duplicate AddListener without RemoveListener on disable
- **Fix:** Register-once pattern + RemoveListener in OnDisable; assert at boot

### 7. RESTYLE Spam
- **Symptom:** RESTYLE log every frame
- **Cause:** Restyle called in Update() not idempotent
- **Fix:** Idempotent restyle (boot + theme change only); verify 60s play silence

### 8. Shader Compile Errors
- **Not handled by this skill** — escalate to shader specialist
- **Pattern:** Shader compiler errors, variant limits exceeded

## Regex Patterns for Classification

```regex
# Duplicate-tree
CS0101|CS0111|CS0229|CS0462|CS8646

# Plugin conflicts
Multiple plugins.*sqlite3

# Runtime exceptions
NullReferenceException|MissingReferenceException

# Listener mismatch
Listeners? \d+≠\d+|listener count mismatch

# RESTYLE
RESTYLE|restyle.*every frame

# Deprecation
CS0618
```

## Evidence Line Format (Appendix A)

```
<path|object> | slot/mesh/collider | cause=<duplicate-type|missing-plugin|wiped-static|dup-listener|restyle-spam> | evidence=<console-export|grep|shot> | decision=<relink|quarantine|watchdog|register-once|idempotent> | phase=<P0>
```