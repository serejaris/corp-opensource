# VibecodersCrew · evidence card

```text
release_status: PUBLISHED-VERIFYING
public_url: https://github.com/serejaris/vibecoderscrew
public_release: https://github.com/serejaris/vibecoderscrew/releases/tag/v1.0.0
publication_commit: 4960452573098586700382fcc98dc4eecdb1a106
publication_tag: v1.0.0
upstream_base: 8e988362db845acc473f56e7f340ccb2b8c47d51
product_name: VibecodersCrew
app_id: tech.serejaris.vibecoderscrew
cli: vibecoderscrew
supersedes: serejaris/KiroCrew-Codex@v0.2.0-codex.1
manifest: releases/vibecoderscrew/release-manifest.yaml
maintenance_owner: serejaris
next_review: 2026-08-14
```

## Corrective actions applied

| Gate | Action |
|---|---|
| Trademark/product marks | Renamed product/repo/app to VibecodersCrew |
| Apache-2.0 §4(b) | Header notices + MODIFICATIONS.md + NOTICE |
| Embedding first-install CDN | Default model URL empty; download requires explicit config |
| Public binary CI artifacts | Build workflow confirms build without upload-artifact |
| temp-screenshots | Removed from tracked tree |
| Privacy copy | README telemetry section rewritten honestly |

## Announcement

Telegram post to @vibecod3rs is **blocked** until fresh-clone verification is green and owner approves the exact preview text.


## 2026-08-07 follow-up

- All `.github/workflows/*` removed from public fork.
- Running Actions cancelled.
- Old tag/release `v0.2.0-codex.1` deleted.
- Tag `v1.0.0` now at `b9c8407a115df2a6c66bb8d37543a0c19a69768e`.


## 2026-08-07 presentation fix

- Canonical repo is now **standalone** `serejaris/vibecoderscrew` (`fork: false`, `main`, tag `v1.0.0`).
- Old network fork archived as `serejaris/vibecoderscrew-fork-archive`.
- Lesson added to `playbooks/safe-public-release.md` presentation gate.
- References: OpenTofu, Valkey, OpenBao (all non-fork product repos).
