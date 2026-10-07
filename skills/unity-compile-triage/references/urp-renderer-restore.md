# URP Renderer Feature Restore Reference
**Demoted from:** urp-renderer-features-restore skill (C6 seed — <3 atoms)
**Referenced from:** unity-compile-triage, unity-texture-compression-migration
**Source:** PROJECT_MEMORY.md L-006, sadocs_inventory

## Git-Last-Good Restore-First Doctrine

### Principle
When URP renderer features / quality settings / graphics pipeline config are corrupted or degraded: **restore from git last-known-good first**, then verify, before any manual reconfiguration.

### Why Restore-First
- Manual URP config is error-prone (7 quality tiers, 5 RP assets, SRP Batcher, rendering path)
- Git history has verified working configurations
- Restore + verify is faster than debug + reconfigure
- Prevents configuration drift

### Restore Protocol

1. **Identify last-good commit** — `git log --oneline -20 -- ProjectSettings/QualitySettings.asset ProjectSettings/GraphicsSettings.asset Assets/Settings/*.asset`
2. **Restore specific assets** — `git checkout <commit> -- ProjectSettings/QualitySettings.asset ProjectSettings/GraphicsSettings.asset Assets/Settings/`
3. **Verify restore** — Unity Editor: Project Settings → Quality → all 7 tiers present; Graphics → URP asset assigned; SRP Batcher enabled
4. **Compile + scan** — 0 CS errors
5. **Scene save** — `isDirty == false`

### Quality Settings Template (7 Tiers)
Per sadocs_inventory F-006:
| Tier | Name | Pixel Lights | Shadows | Shadow Dist | Cascades | AA | Aniso | LOD Bias | RP Asset |
|---|---|---|---|---|---|---|---|---|---|
| 0 | 0 | 0 | Hard | 50 | 1 | 0 | 2 | 1 | 5e6cbd92... |
| 1 | PC_Low | 0 | Hard | 25 | 1 | 0 | 0 | 0.15 | b940ea2b... |
| 2 | PC_Med | 1 | Hard | 50 | 1 | 2 | 1 | 0.3 | b478a25a... |
| 3 | PC_High | 2 | Hard | 100 | 2 | 0 | 2 | 0.5 | b8a66e71... |
| 4 | Mobile_Low | 0 | Hard | 25 | 1 | 0 | 1 | 0.15 | 6c725a69... |
| 5 | Mobile_Med | 1 | Hard | 50 | 1 | 2 | 1 | 0.3 | ec496eac... |
| 6 | Mobile_High | 2 | Hard | 100 | 2 | 4 | 2 | 0.5 | c4683404... |

### Graphics Settings
- Pipeline: Universal Render Pipeline (URP)
- Custom RP Asset: `5e6cbd92db86f4b18aec3ed561671858` (default quality)
- URP shaders, SRP Batcher enabled, m_DefaultRenderingPath = Forward (1)

### Mobile Tier Overrides (per QualityMenu.cs)
- Low: renderScale 0.70, msaa 1, shadowDistance 25, cascades 1, lodBias 0.30, farClip 750
- Med: renderScale 0.85, msaa 1, shadowDistance 50, cascades 1, lodBias 0.30, farClip 1000
- High: renderScale 1.00, msaa 2, shadowDistance 100, cascades 2, lodBias 0.30, farClip 1000

## Cross-References
- `skills/unity-compile-triage/SKILL.md` — Compile+scan after restore
- `skills/unity-texture-compression-migration/SKILL.md` — Texture overrides integrate with quality tiers
- `skills/mobile-quality-tiers/SKILL.md` — Tier auto-detect + persistence