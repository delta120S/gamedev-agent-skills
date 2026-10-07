# Prompt Pattern: forensic-cleanup
**When to Use:** Duplicate asset trees, dead files, orphan refs, spaced names, typo-named duplicates
**When NOT to Use:** Intentional prefab instances, legitimate LOD variants, different texture slots, runtime-only cleanup

## Template
```markdown
# OPERATION FORENSIC-CLEANUP — <project> | autonomous | all permissions pre-granted

## 0. ROLE & AUTONOMY
[0-01] Senior Unity tech artist + repo surgeon; unattended; no questions; finish or BLOCKED-final.
[0-02] Evidence-only claims; MINIMAL-DIFF: smallest fix per defect; relink canonical first.
[0-03] QUARANTINE law for replaced assets; QUARANTINE.md ledger entry per delete.

## 1. MISSION & SUCCESS
[1-01] Eliminate all duplicate asset groups (content-hash + material signature).
[1-02] Remove zero-inbound-reference assets (dead files, Blender sources, recovery scenes).
[1-03] Fix spaced names + typo names (canonical unspaced/correct spelling).
[1-04] SUCCESS = 0 duplicate groups, 0 zero-ref assets (except EC4 keep-rows), compile clean, scene saves clean.

## 2. CONSTITUTION
[C-01] MINIMAL-DIFF: relink canonical assets first (git last-good or project texture sets).
[C-02] BATCH law: relink-then-delete batches ≤50; compile+scan between; commit each.
[C-03] EVIDENCE law: no change without Appendix-A evidence line.
[C-04] SCENE law: explicit SaveScene; isDirty false after.
[C-05] QUARANTINE law: reversible deletes + QUARANTINE.md ledger.
[C-06] GIT law: tag pre-<phase> before destructive batches; commit per gate; no rewrite.

## PHASE 1 — INVENTORY & CLASSIFY (30m)
[P1-01] Content-hash scan: textures/meshes/audio → groups G1-G<N>.
[P1-02] Material signature scan: shader + texture set → groups M1-M<N>.
[P1-03] Prefab structure scan: identical hierarchy → groups P1-P<N>.
[P1-04] Zero-ref scan: textures, meshes, scenes, scripts, assets with 0 inbound refs.
[P1-05] Classify each group: INTENTIONAL (exclude) vs TRUE DUPLICATE (process).
[G1-1] GATE: inventory complete; classification logged; canonical hints assigned.

## PHASE 2 — RELINK & DELETE (60m, batched ≤50)
[P2-01] Per group: determine canonical (most inbound refs; tie → oldest path).
[P2-02] Relink all refs to canonical; delete duplicate; verify refs intact.
[P2-03] Special cases: spaced names → delete spaced; typos → rename canonical + relink.
[P2-04] Zero-ref assets: Blender sources → _SA_DOCS/; recovery scenes → quarantine; screenshots → _SA_DOCS/captures/.
[P2-05] After each batch: reference scan + compile + scene save.
[G2-1] GATE: 0 duplicate groups; 0 zero-ref (except keep-rows); compile clean.

## PHASE 3 — VERIFICATION (15m)
[P3-01] Re-scan content hashes + material signatures: 0 true duplicates.
[P3-02] Compile + play 60s: 0 errors, 0 missing refs.
[P3-03] Scene saves clean (isDirty==false).
[G3-1] GATE: all verification green; commit + tag forensic-cleanup-done.

## ANTI-LOOP
[A-01] Retry ≤3 with change; identical command never 4th; same output twice = switch strategy.
[A-02] Fix ≤3/defect; verify ≤4/phase; two zero-delta cycles = LOOP → strategy change.
[A-03] SCAN-ONCE; gated never re-run; blocked 3× same cause → BLOCKED doc, move on.

## APPENDIX A — EVIDENCE LINE
<path|object> | type | inbound_refs=<n> | evidence=<hash-scan|mat-sig-scan|orphan-scan> | decision=<relink-then-delete|move|quarantine|keep+reason> | reason=<one line> | phase=<P#>

## SKILLS REQUIRED
- unity-duplicate-tree-surgery (primary)
- unity-compile-triage (compile+scan gates)
- git-hygiene-for-unity (pair-move, GUID preservation, tag/commit)
- quarantine-protocol (reversible deletes, ledger)

## PLACEHOLDERS
- <project>: Unity project name
- <N>: group counts (discovered at P1)
- <stable-path>: staged-scripts dir on a stable drive (per L-103, never %TEMP%)
```

## Provenance
- DUPLICATES.md (19 texture groups, 1 prefab group, 116 material groups)
- DEAD_FILES.md (35+ zero-ref assets with decisions)
- PROJECT_MEMORY.md L-68, L-79, L-87, L-103, L-112, L-113
- QUARANTINE.md (7 entries)