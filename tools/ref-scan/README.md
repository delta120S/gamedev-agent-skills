# ref-scan — AssetDatabase Reverse Dependency Scanner
**Purpose:** AssetDatabase.GetDependencies reverse lookup for canonical selection in duplicate surgery
**Source:** DUPLICATES.md canonical rule, PROJECT_MEMORY.md L-68
**Provenance:** sadocs_duplicates F-012 (canonical rule: most inbound refs)
**Tested:** Yes (2026-10-06 duplicate audit)
**Sandbox-Safe:** Yes (Unity Editor API only)

## Usage
```csharp
// Get all assets that depend on target asset
var allAssets = AssetDatabase.FindAssets(""); // all GUIDs
var dependents = new List<string>();
foreach (var guid in allAssets) {
    var path = AssetDatabase.GUIDToAssetPath(guid);
    var deps = AssetDatabase.GetDependencies(path, recursive: true);
    if (deps.Contains(targetPath)) {
        dependents.Add(path);
    }
}
return dependents.Count; // inbound ref count
```

## Canonical Selection (Binding)
1. **Most inbound refs** — measured at execution via reverse lookup above
2. **Tie → oldest path** — `File.GetCreationTimeUtc`
3. **Never delete canonical member**

## Integration
- Called by `unity-duplicate-tree-surgery` skill step 3
- Called by `guid-swap` tool for inbound ref counting
- Results logged in DUPLICATES_REPORT.md per group

## Exit Codes
- 0: Scan complete, counts returned
- 1: AssetDatabase not ready (compiling)
- 2: Target asset not found