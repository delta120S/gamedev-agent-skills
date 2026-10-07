# Serialized Asset Upgrade Reference
**Demoted from:** unity-serialized-upgrade skill (C4 seed — <3 atoms)
**Referenced from:** unity-compile-triage, unity-duplicate-tree-surgery
**Source:** PROJECT_MEMORY.md L-007, sadocs_inventory

## ForceReserializeAssets Protocol

### When to Use
- After major Unity version upgrade
- After package upgrades that change serialization format
- After script namespace/assembly changes
- Before major builds to ensure clean serialization

### Batch Processing (≤50 assets per batch)
```csharp
// Editor script pattern
var assets = AssetDatabase.FindAssets("t:Prefab t:ScriptableObject t:Material");
var batch = assets.Take(50).ToArray();
foreach (var guid in batch) {
    var path = AssetDatabase.GUIDToAssetPath(guid);
    AssetDatabase.ImportAsset(path, ImportAssetOptions.ForceUpdate | ImportAssetOptions.ForceSynchronousImport);
}
AssetDatabase.SaveAssets();
// Compile + scan between batches
```

### Diff Sanity Check
- `git diff --stat` after each batch — expect only serialization format changes (m_ObjectHideFlags, serialized field order)
- No logic changes — if logic diffs appear, STOP and investigate
- Commit per batch with pathspec-limited `git add`

### Durability Proof (per L-112)
- Mesh-level pre-bake: `PhysicsEditorMeshExtensions.SetPreBakeCollisionMesh(mesh, isConvex, true)` — survives SaveAssets+Refresh
- Importer-level (STORE): `ModelImporter.SetPreBakeCollisionMesh(isConvex, true)` writes `preBakeTriangleCollisionMesh: 1` into `.meta`
- Next import re-bakes it (`HasPreBakeCollisionMesh` true on mesh)
- **Blast radius counted in ASSETS not meshes**: 321 distinct missing meshes from just 5 files

### Self-Heal AssetPostprocessor
```csharp
class PreBakeCollisionGuard : AssetPostprocessor {
    void OnPreprocessModel() {
        if (ShouldPreBake(importer)) {
            importer.SetPreBakeCollisionMesh(true);
        }
    }
}
```
Map persisted in `ProjectSettings/PreBakeCollisionMap.txt`

## Cross-References
- `skills/unity-compile-triage/SKILL.md` — Compile+scan between batches
- `skills/unity-duplicate-tree-surgery/SKILL.md` — GUID preservation during moves
- `skills/git-hygiene-for-unity/SKILL.md` — Tag/commit protocol