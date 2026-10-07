# dup-hash-scanner — Content-Hash Duplicate Asset Scanner
**Purpose:** Scan for content-hash identical asset groups (textures, meshes, audio) and material signature groups
**Source:** DUPLICATES.md G1-G19 (19 texture/mesh/audio groups), M1-M20 (116 material groups)
**Provenance:** sadocs_duplicates F-012, F-013, F-015
**Tested:** Yes (2026-10-06 duplicate audit)
**Sandbox-Safe:** Yes (read-only file hashing + Unity AssetDatabase)

## Usage
```csharp
// Texture/Mesh/Audio content hash scan
var hashes = new Dictionary<string, List<string>>();
foreach (var guid in AssetDatabase.FindAssets("t:Texture2D t:Mesh t:AudioClip")) {
    var path = AssetDatabase.GUIDToAssetPath(guid);
    var hash = ComputeSHA256(path); // file content hash
    if (!hashes.ContainsKey(hash)) hashes[hash] = new List<string>();
    hashes[hash].Add(path);
}
// Groups with Count > 1 = duplicates

// Material signature scan (shader + texture set)
var matSigs = new Dictionary<string, List<string>>();
foreach (var guid in AssetDatabase.FindAssets("t:Material")) {
    var mat = AssetDatabase.LoadAssetAtPath<Material>(AssetDatabase.GUIDToAssetPath(guid));
    var sig = mat.shader.name + "|" + string.Join(",", mat.GetTexturePropertyNames()
        .Select(p => mat.GetTexture(p) ? AssetDatabase.AssetPathToGUID(AssetDatabase.GetAssetPath(mat.GetTexture(p))) : "null"));
    if (!matSigs.ContainsKey(sig)) matSigs[sig] = new List<string>();
    matSigs[sig].Add(AssetDatabase.GUIDToAssetPath(guid));
}
```

## Duplicate Categories (from DUPLICATES.md)
- **G1-G19:** 19 content-hash identical texture/mesh/audio groups
- **P1:** 1 structurally identical prefab group
- **M1-M20:** 116 near-identical material signature groups (M1=89 URP/Lit no textures = KEEP+REASON)

## Special Handling
- **M1 (89 no-texture URP/Lit):** KEEP+REASON — FBX-import defaults
- **M14 (Sofa fabric):** KEEP — different texture slots (Base/Height/Normal)
- **M15 (House ceramic):** KEEP — different source textures
- **Spaced names (G15, M3, M17):** Delete spaced variant
- **Typos (M4, M16):** Rename canonical + relink

## Exit Codes
- 0: Scan complete, groups catalogued
- 1: No duplicates found
- 2: Hash computation failed