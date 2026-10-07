# guid-swap — GUID + Meta Surgery Utility
**Purpose:** Inbound-refs counting + scripted GUID swaps + post-scan proof
**Source:** Tools/fixv4_p1_edits.py, PROJECT_MEMORY.md L-68, L-112, L-113
**Provenance:** PROJECT_MEMORY.md L-68 (pair-move + GUID-compare), L-112 (importer-level STORE), L-113 (static batching flags)
**Tested:** Yes (2026-09-24, 2026-09-30)
**Sandbox-Safe:** Yes (Unity Editor API only)

## Usage
```python
# Count inbound refs for asset
refs = AssetDatabase.GetDependencies(assetPath, recursive=True)
inbound = [d for d in allAssets if assetPath in AssetDatabase.GetDependencies(d)]

# Pair-move file + .meta (preserves GUID)
File.Move(src, dst)
File.Move(src + ".meta", dst + ".meta")
# Verify GUID identical
assert AssetDatabase.AssetPathToGUID(dst) == originalGUID

# Load-verify
AssetDatabase.LoadAssetAtPath<Object>(dst) != null
```

## Protocol (L-68, L-112)
1. **Count inbound refs** before any move/swap
2. **Pair-move** file + .meta together (preserves GUID)
3. **GUID-compare** — verify identical after move
4. **Load-verify** — `AssetDatabase.LoadAssetAtPath` succeeds
5. **Importer-level STORE** for durable flags: `ModelImporter.SetPreBakeCollisionMesh(isConvex, true)` writes `preBakeTriangleCollisionMesh: 1` into `.meta`
6. **Prove durability** — force reimport (`ImportAsset(path, ForceUpdate)`) → `HasPreBakeCollisionMesh==True`

## Exit Codes
- 0: Swap verified (GUID identical, load succeeds, refs updated)
- 1: GUID mismatch after move
- 2: Load-verify failed
- 3: Inbound refs not updated

## Key Lessons
- **L-68:** Bracket paths break Unity AssetDatabase ops → pair-move + GUID-compare + load-verify replaces tool-trust
- **L-112:** Mesh-level pre-bake is CACHE, importer-level is STORE — prove durability with FORCED reimport
- **L-113:** Build-time effects invisible in editor — measure authoring FLAGS (`GameObjectUtility.GetStaticEditorFlags`), not runtime property