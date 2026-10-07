---
name: Unity Texture Compression Migration (PVRTC→ASTC)
description: |
  When project textures use legacy PVRTC compression, need per-tier ASTC 6x6 overrides, sRGB discipline enforcement, or chunked reimport to avoid reimport storms
  Trigger phrases: "PVRTC", "ASTC 6x6", "texture compression", "sRGB discipline", "reimport storm", "platform overrides"
  Not for: runtime texture streaming, mesh compression, audio compression, shader variant optimization
license: MIT
provenance: 4 sources
confidence: verified
created: 2026-10-07
maintainer: agent-skills
draft: false
---

## WHEN
- Textures imported with legacy PVRTC format (obsolete on modern mobile)
- Need per-quality-tier ASTC 6x6 platform overrides (Android)
- sRGB discipline violations (normals imported as sRGB, albedos not)
- Reimport storm risk when processing hundreds of textures
- Platform-specific override configuration needed

## WHY
The OPTIM-V2 mission [src:project_memory#F-018:L-093] migrated 358 maps textures to Android ASTC 6x6 (textureFormat: 50) with maxsize caps (2048 hero / 1024 game) + streamingMipmaps=1, saving ~849.57 MB platform import footprint without mutating source textures. sRGB rules strictly preserved (64 normals sRGB=0 / 294 albedos sRGB=1). Chunked processing in batches ≤50 prevents reimport storm. PVRTC is obsolete on modern Android — ASTC 6x6 is the standard.

## PROCEDURE

1. **Audit current texture formats** — scan all Texture2D assets for PVRTC, ETC1, ETC2 formats; catalog by folder and usage [src:project_memory#F-018:L-093]
2. **Configure per-tier ASTC 6x6 overrides** — Android platform settings: textureFormat=50 (ASTC 6x6), maxsize 2048 (hero) / 1024 (game), streamingMipmaps=1 [src:project_memory#F-018:L-093]
3. **Enforce sRGB discipline** — normals: sRGB=0 (64 textures); albedos: sRGB=1 (294 textures); audit violations == 0 [src:project_memory#F-018:L-093]
4. **Chunked reimport in batches ≤50** — process textures in batches of 50 to prevent reimport storm; compile+scan between batches [src:project_memory#F-018:L-093]
5. **Verify platform import footprint** — measure Android build texture memory before/after; target ~849 MB savings [src:project_memory#F-018:L-093]
6. **Spot-check visual fidelity** — compare key textures in Scene/Game view; no visible degradation [src:project_memory#F-018:L-093]
7. **Commit per batch** — git add pathspec-limited; tag per phase [src:project_memory#F-018:L-079]

## PITFALLS

| Symptom | Cause | Fix |
|---|---|---|
| PVRTC textures cause build warnings/errors on Android | PVRTC obsolete on modern Android GPUs | Migrate to ASTC 6x6 per-tier overrides [src:project_memory#F-018:L-093] |
| sRGB violations: normals look wrong, albedos too dark/bright | Normals imported with sRGB=1, albedos with sRGB=0 | Enforce: normals sRGB=0, albedos sRGB=1; audit violations == 0 [src:project_memory#F-018:L-093] |
| Reimport storm freezes Editor for minutes | Processing 358+ textures in single batch | Chunked batches ≤50; compile+scan between batches [src:project_memory#F-018:L-093] |
| Source textures mutated on disk | Platform overrides applied to source instead of platform-specific | Use TextureImporter.platformTextureSettings for Android-only overrides [src:project_memory#F-018:L-093] |
| Hero textures (4K) exceed maxsize cap | Maxsize 2048 cap downscales hero textures | Identify hero textures (skybox, key UI) and exclude from cap or use 4096 [src:project_memory#F-018:L-093] |
| ASTC 6x6 not supported on older devices | Some older GPUs lack ASTC support | Fallback tier: keep PVRTC for Low tier devices only; Medium+ use ASTC |

## VERIFY

| Check | Command / Method | Expected Evidence | Fail Action |
|---|---|---|---|
| PVRTC count | Scan Texture2D.format | 0 PVRTC textures | Process remaining |
| ASTC 6x6 overrides | TextureImporter.platformTextureSettings[Android] | format=50, maxsize set, streamingMipmaps=1 | Re-apply overrides |
| sRGB audit | Scan normals (sRGB=0) + albedos (sRGB=1) | Violations == 0 | Fix violations |
| Import footprint | Android build texture memory | ~849 MB savings vs baseline | Investigate outliers |
| Visual spot-check | Scene/Game view key textures | No visible degradation | Revert specific textures |
| Reimport storm | Batch size during processing | ≤50 textures per batch | Reduce batch size |

## ESCALATE

| Stop Condition | Handoff Skill | Human Ask |
|---|---|---|
| >50 sRGB violations after audit | — | Art review needed: which textures are normals vs albedos |
| Hero texture quality unacceptable at 2048 | — | Art decision: identify hero textures for 4096 exception |
| Older device tier needs PVRTC fallback | `mobile-quality-tiers` | Configure Low tier PVRTC, Medium+ ASTC |

## REFERENCES

- `references/astc-override-config.md` — TextureImporter platform settings code + batch script
- `references/srgb-audit.md` — sRGB validation script + violation catalog
- `references/batch-reimport.md` — Chunked ≤50 batch processor with compile+scan gates
- `mobile-quality-tiers` - Tier configuration integration (see skills catalog)
- `skills/unity-compile-triage/SKILL.md` - Compile+scan between batches