---
name: Unity Duplicate Asset/Object Surgery
description: |
  When project has duplicate assets (content-hash identical textures/meshes/audio), near-identical materials (color variants), structurally identical prefabs, or zero-inbound-reference assets cluttering the project
  Trigger phrases: "duplicate assets", "duplicate textures", "duplicate materials", "spaced file names", "zero reference assets", "canonical selection", "relink duplicates"
  Not for: intentional prefab instances in scene, legitimate LOD variants, different texture slots (Base/Height/Normal), different source textures
license: MIT
provenance: 5 sources
confidence: verified
created: 2026-10-07
maintainer: agent-skills
draft: false
---

## WHEN
- Duplicate texture groups found (same content hash, different paths) — e.g., UCB_GLASS_CLEAN_dif.png ×4 across car variants
- Near-identical material signature groups — same shader + same texture set, different color variants (116 groups found)
- Structurally identical prefabs — same hierarchy, same components, different paths
- Zero-inbound-reference assets — textures, meshes, scenes, scripts with no dependencies
- Spaced file names causing duplicate detection — e.g., "Road_17_PARKING_001 1.jpg" vs "Road_17_PARKING_001.jpg"
- Typo-named duplicates — e.g., "Aluminiuim" vs "Aluminium"

## WHY
Duplicate assets waste disk space (1.38 GB Blender sources, 19 texture groups, 116 material groups), increase build size, cause confusion during maintenance, and risk inconsistent fixes. The canonical-by-refs rule [src:sadocs_duplicates#F-012] ensures the most-used asset survives. Spaced names and typos break tooling and version control. Zero-ref assets are dead weight — Blender sources belong in `_SA_DOCS/`, recovery scenes in quarantine.

## PROCEDURE

1. **Scan for duplicate groups** — content-hash scan for textures/meshes/audio; material signature scan (shader + texture set) for near-duplicates; prefab structure comparison for identical prefabs [src:sadocs_duplicates#F-012]
2. **Classify each group** — intentional instances (vegetation, props, buildings) = EXCLUDE; true duplicates = PROCESS [src:sadocs_duplicates#F-016]
3. **Determine canonical per group** — most inbound refs (AssetDatabase.GetDependencies reverse lookup); tie → oldest path (File.GetCreationTimeUtc) [src:sadocs_duplicates#F-012]
4. **Relink-then-delete non-canonical** — for each duplicate: update all references to canonical, delete duplicate, verify refs intact [src:sadocs_duplicates#F-012]
5. **Handle special cases**:
   - Spaced names: delete spaced variant, keep unspaced [src:sadocs_duplicates#F-013:G15]
   - Typos: rename canonical, relink duplicates [src:sadocs_duplicates#F-013:M16]
   - Different texture slots (M14) or source textures (M15): KEEP both [src:sadocs_duplicates#F-015]
6. **Zero-ref assets** — Blender sources (5 files, 1.38 GB) → move to `_SA_DOCS/` [src:sadocs_dead#F-017:A1-A5]; Recovery scenes (7) → quarantine [src:sadocs_dead#F-017:A6-A8,A12]; Screenshots (18) → extract to `_SA_DOCS/captures/` [src:sadocs_dead#F-017:A13]
7. **After each group**: reference scan + compile + scene save [src:sadocs_duplicates#F-016]
8. **Every 5 groups**: Scene-view screenshot + one-line judge [src:sadocs_duplicates#F-016]
9. **Never delete canonical member** [src:sadocs_duplicates#F-012]
10. **Quarantine deleted files** → `_SA_QUARANTINE/P2/` with QUARANTINE.md ledger entry [src:sa_quarantine#F-001]
11. **Verify**: Compile clean, no missing refs, scene saves clean (isDirty==false)

## PITFALLS

| Symptom | Cause | Fix |
|---|---|---|
| "Couldn't set project path" on spaced paths | PowerShell glob treats [] as char class | Use `-LiteralPath` + `git -C` [src:project_memory#F-018:L-015] |
| manage_asset move reports failure AFTER moving files on disk | Unity AssetDatabase ops break on bracket paths | Pair-move + GUID-compare + load-verify replaces tool-trust [src:project_memory#F-018:L-068] |
| Deleting canonical member breaks 52 asset refs | Blender sources have 52 GUID refs across project | EC4 keep-row: probe found 52 GUID ref files; moving breaks them [src:project_memory#F-018:L-063] |
| Build deletes tracked files mid-build | Android build mutates tree (PerformanceTestRun*.json) | `git status --porcelain` BEFORE/AFTER build; `git checkout --` deleted files [src:project_memory#F-018:L-087] |
| Background archiver deletes .md mid-turn | Root cleanup race condition | `Test-Path` before every edit + immediate commit per phase [src:project_memory#F-018:L-065] |
| Orphan .meta after delete | File deleted without .meta | Pair-move file+.meta together; verify pairing clean [src:sadocs_dead#F-017:E] |

## VERIFY

| Check | Command / Method | Expected Evidence | Fail Action |
|---|---|---|---|
| Compile errors | Unity Console export | 0 CS errors | Re-run reference scan |
| Missing refs | `AssetDatabase.GetDependencies` reverse | 0 missing after relink | Restore canonical from quarantine |
| Scene dirty | `EditorSceneManager.IsDirty()` | false | Save scene |
| Duplicate groups | Re-scan content hashes | 0 true duplicate groups | Process remaining |
| Quarantine ledger | QUARANTINE.md entries | 1 per deleted group | Log missing entries |

## ESCALATE

| Stop Condition | Handoff Skill | Human Ask |
|---|---|---|
| Blender sources have inbound refs (52 GUID refs) | — | Do NOT move; keep in Assets/ with EC4 keep-row |
| >3 fix cycles on same duplicate group | `git-hygiene-for-unity` | Only if relink fails repeatedly |
| Unresolvable canonical (tie + same age) | — | Manual decision required |

## REFERENCES

- `references/duplicate-tree-protocol.md` — Full protocol with canonical rules, quarantine, GUID preservation
- `references/zero-ref-catalog.md` — Complete catalog of zero-ref assets with decisions
- `skills/unity-compile-triage/SKILL.md` - Handoff for duplicate-tree compiler errors
- `git-hygiene-for-unity` - Tag/commit protocol, pair-move GUID preservation (see skills catalog)
- `quarantine-protocol` - Quarantine ledger format and prune policy (see skills catalog)