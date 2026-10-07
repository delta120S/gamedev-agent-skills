# blender-export — Unity→Blender Mesh Export Utility
**Purpose:** Export unique meshes + instance/transform table in line-based format (not JSON), verify byte count
**Source:** PROJECT_MEMORY.md L-139 (EXPORT lesson)
**Provenance:** PROJECT_MEMORY.md L-139
**Tested:** Yes (2026-10-01)
**Sandbox-Safe:** Yes (read-only Unity API, writes to staged path)

## Usage
```csharp
// Editor script: Assets/Editor/ExportForBlender.cs
// Menu: Tools > Export > Blender Mesh Export
// Output: <stable-path>/blender-export/unique_meshes.mesh + instances.transform
```

## Export Format (Line-Based, Not JSON)
```
# UNIQUE_MESHES
mesh_guid|vertex_count|triangle_count|bounds_min|bounds_max
<guid>|<verts>|<tris>|<x,y,z>|<x,y,z>
...
# INSTANCES
instance_id|mesh_guid|position|rotation|scale|parent_id
<id>|<guid>|<x,y,z>|<x,y,z,w>|<x,y,z>|<parent>
...
```

## Verification
- Exported byte count within 1% of `uniqueVerts * 32 + uniqueTris * 12`
- File count inside `Assets/` unchanged by export
- Blender imports mesh + instance table → reconstructs scene hierarchy

## Exit Codes
- 0: Export complete, byte count verified within 1%
- 1: No unique meshes found (empty selection)
- 2: Byte count mismatch (export incomplete)
- 3: Blender reimport verification failed

## Key Lessons (L-139)
- `Buffer.BlockCopy` throws on `Vector3[]`/`Vector2[]` (structs not primitives) → convert to `float[]` first
- `int[]` indices can be copied directly
- Geometry must be REUSED, never baked: CityMap_Main.unity = 13,688 MeshFilters but only 579 unique meshes / 9,134,863 unique verts vs 25,951,291 if flattened (~2.4 GB OBJ)
- Deliberately not JSON — string escaping cannot corrupt line-based format
- Let Blender do matrix/axis conversion where screenshot can verify