# Duplicate Tree Surgery Protocol
**Referenced from:** `skills/unity-compile-triage/SKILL.md` step 2, ESCALATE
**Source:** DUPLICATES.md, DEAD_FILES.md, PROJECT_MEMORY.md L-068, L-079, L-087

## Canonical Selection Rule (Binding)

1. **Most inbound references** — measured at execution via `AssetDatabase.GetDependencies` reverse lookup
2. **Tie → oldest path** — earliest `File.GetCreationTimeUtc`
3. **Never delete a canonical member**

## Duplicate Categories & Handling

### Content-Hash Identical Assets (19 groups, G1-G19)
- Textures: UCB_GLASS_CLEAN_dif.png ×4, plant05_normal.png ×2, Numberplates_dif.png ×2, etc.
- Meshes: ExitCar.fbx == ExitCar_RightSide.fbx (3.28 MB), EnterCar.fbx triple (3.30 MB)
- **Action:** Relink-then-delete non-canonical; verify refs after each group

### Structurally Identical Prefabs (1 group, P1)
- Floor0Model.prefab (BSystem/Resources) == FloorModel_00.prefab (Models/Staircase/prefabs)
- **Canonical:** BSystem/Resources (Resources.Load path)
- **Action:** Relink-then-delete duplicate

### Near-Identical Materials (116 signature groups, M1-M20)
- **M1 (89):** URP/Lit no textures — **KEEP+REASON** (FBX-import defaults)
- **M2-M13, M16-M20:** Color variants of same texture set — relink-then-delete
- **M14-M15:** Different texture slots/source textures — **KEEP**

### Same-Mesh Scene Objects (Deferred)
- Vegetation/prop/building instances — **EXCLUDED** (intentional prefab instances)
- Only flag if: same mesh+material+transform within 0.01 epsilon AND no unique gameplay script state AND not required prefab instance

## Execution Protocol (Phase 6)

1. Process groups ascending risk: unreferenced duplicates first (G15, P1), then relink-then-delete
2. After each group: reference scan + compile + scene save
3. Every 5 groups: Scene-view screenshot + one-line judge
4. Never delete canonical member
5. DUPLICATES_REPORT.md per group: action | refs before/after | verification | commit

## Quarantine Protocol

- Quarantined files → `D:\[game]\gameadv\_SA_QUARANTINE\P2\` (or P9/P10)
- QUARANTINE.md ledger entry per [P1-02] format
- Prune policy: 30 days after verification green, or next major Unity version upgrade

## GUID Preservation

- Filesystem file+.meta pair moves preserve GUIDs (verified identical + pipeline resolves) [src:project_memory#F-018:L-068]
- Always verify on-disk result + GUIDs after tool errors
- `git add -A` with rename-detect 100% [src:project_memory#F-018:L-079]

## Build Artifact Protection

- Android build DELETES tracked files mid-build: PerformanceTestRunInfo.json, PerformanceTestRunSettings.json (+.meta) [src:project_memory#F-018:L-087]
- Rule: `git status --porcelain` BEFORE and AFTER every build; `git checkout --` anything build deleted before claiming clean tree