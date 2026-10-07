# PUBLISH_REPORT.md — v1.0.0 + Brand Assets Publish

**Date:** 2026-10-07
**Operator:** autonomous publish run (GITHUB PUBLISH ORDER)

## Repository

| Field | Value |
|---|---|
| Repo URL | https://github.com/delta120S/gamedev-agent-skills |
| Local path | `D:\game\gameadv\_SA_TOOLING\skillforge-repo` |
| Branch | `main` (fast-forward pushed, no force, no history rewrite) |
| Tag | `v1.0.0` (annotated, pushed) |
| Release URL | https://github.com/delta120S/gamedev-agent-skills/releases/tag/v1.0.0 |
| Description | Provenance-first AI-agent skills for Unity game development — distilled from a full production project |
| Topics | `unity`, `game-development`, `ai-agents`, `agent-skills`, `claude`, `opencode`, `knowledge-management` |

## Asset table

| File (repo path) | Dimensions | Aspect | Role |
|---|---|---|---|
| `docs/assets/banner-repo.png` | 3584×1184 | 3.03 ≈ 3:1 | Full-width README header banner |
| `docs/assets/hero-poster.png` | 2752×1536 | 1.79 ≈ 16:9 | Gallery hero poster |
| `docs/assets/emblem-logo.png` | 2752×1536 | 1.79 | Source emblem (Gallery + release asset) |
| `docs/assets/emblem-256.png` | 256×143 | 1.79 | Inline About-section logo (derived, resized) |
| `docs/assets/social-preview-1280x640.png` | 1280×640 | 2:1 | Social preview (center-crop of banner; web-UI upload) |

Source files (chosen set): `C:\Users\Administrator\Desktop\New folder (2)\{banner-repo,hero-poster,emblem-logo}.png`
— newest 48h image set matching required ratios (created 2026-10-07 15:05–15:06).
Derivation tool: **python-PIL 12.3.0** (`magick` and `ffmpeg` not installed).
Metadata: no `tEXt`/`iTXt`/`zTXt` chunks present in sources; all outputs re-saved through PIL (provenance-safe).

> Note: emblem source is 16:9 rather than square/oval; used as-is (center of mark is legible at
> 256 px width in the About section). Square avatar crop instructions are in `docs/PUBLISH_NOTES.md`.

## Verification outputs

```
gh repo view --json url,name,description,repositoryTopics
  url         = https://github.com/delta120S/gamedev-agent-skills
  name        = gamedev-agent-skills
  description = Provenance-first AI-agent skills for Unity game development — distilled from a full production project
  topics      = agent-skills, ai-agents, claude, game-development,
                knowledge-management, opencode, unity

raw.githubusercontent.com (HEAD) — all five assets:
  banner-repo.png              -> 200  3992279 bytes
  hero-poster.png              -> 200  5859221 bytes
  emblem-logo.png              -> 200  3775997 bytes
  social-preview-1280x640.png  -> 200   819196 bytes
  emblem-256.png               -> 200    36643 bytes

gh release view v1.0.0
  tag = v1.0.0  url = https://github.com/delta120S/gamedev-agent-skills/releases/tag/v1.0.0
  assets: banner-repo.png (3992279), emblem-logo.png (3775997),
          hero-poster.png (5859221), social-preview-1280x640.png (819196)

Fresh clone (depth 1) into temp:
  docs/assets/ -> 5 files present (all sizes > 0)
  README image links -> all relative: docs/assets/{banner-repo,emblem-256,emblem-logo,hero-poster}.png
  secret scan (tokens / C:\Users\ / D:\game) -> CLEAN

git log:
  <final> chore(publish): v1.0.0 release + brand assets verified
  45b53fe docs(readme): brand header, logo, gallery
  8a0661c docs(assets): brand assets intake (banner/hero/emblem + variants)
  d0698c1 skillforge(P6): repo assembly with catalog and docs [gate: ok]
```

## Scrub log ([0-03])

| File | Change |
|---|---|
| `tools/script-runner/README.md` L11, L28 | Personal path `D:\game\...` → `<PLACEHOLDER>\staged-scripts` (2 occurrences) |

No tokens/keys found in README, release notes, or asset metadata. The provided GitHub PAT was used
only as a process-scoped `GH_TOKEN` and is **not** stored in any repo file.

## Manual steps (required — web UI only)

See **`docs/PUBLISH_NOTES.md`**:

1. **Settings → General → Social preview** → upload `docs/assets/social-preview-1280x640.png`
2. **(Optional)** profile/org **avatar** → upload `docs/assets/emblem-logo.png` (center crop)
