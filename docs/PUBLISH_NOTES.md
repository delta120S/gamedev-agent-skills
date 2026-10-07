# PUBLISH_NOTES.md — Manual GitHub Steps (Web UI Only)

**Repo:** https://github.com/delta120S/gamedev-agent-skills
**Tag / Release:** `v1.0.0` — https://github.com/delta120S/gamedev-agent-skills/releases/tag/v1.0.0
**Date:** 2026-10-07

The following two GitHub settings are **not available via `gh` CLI or the REST API** — they must be
set once in the GitHub web UI.

## 1. Social preview (repository banner image)

1. Open **https://github.com/delta120S/gamedev-agent-skills/settings/general**
   (repo → **Settings** → **General**).
2. Scroll to the **Social preview** section → click **Edit**.
3. Upload **`docs/assets/social-preview-1280x640.png`** (from a fresh clone of this repo, or from
   the `social-preview` release asset of v1.0.0).
4. Crop/position as desired (the image is already 1280×640, 2:1) → **Save**.

This image is what link previews show when the repo URL is shared (X/Twitter, Slack, Discord, etc.).

## 2. Avatar (org or profile)

*Optional — owner-level branding.*

1. Open the avatar editor:
   - **Profile:** https://github.com/settings/profile → **Profile picture** → **Upload a photo**
   - **Organization (if used):** org **Settings** → **Profile** → avatar upload
2. Upload **`docs/assets/emblem-logo.png`** → adjust the crop (emblem is 2752×1536; center the mark)
   → **Save**.

A square-cropped variant can be produced locally with:

```bash
magick docs/assets/emblem-logo.png -gravity center -extent 1536x1536 docs/assets/emblem-avatar-1024.png
```

## Verification checklist

- [ ] Repo landing page shows the social preview under **Settings → General → Social preview**
- [ ] Avatar shows the emblem on the owner profile / org page
- [ ] Release v1.0.0 lists 4 assets: `banner-repo`, `hero-poster`, `emblem-logo`, `social-preview`

## Automated (already done — no action needed)

- Repo created, `main` pushed, description + topics set via API
- Tag `v1.0.0` pushed; release published with the three source images + social preview
- `docs/assets/*` verified on raw.githubusercontent.com (HTTP 200)
- See `docs/PUBLISH_REPORT.md` for the full verification table
