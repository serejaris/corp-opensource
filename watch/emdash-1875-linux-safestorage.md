# Emdash #1875 Linux safeStorage candidate, 2026-05-28

Tracker: [#83](https://github.com/serejaris/corp-opensource/issues/83)
Upstream: [generalaction/emdash#1875](https://github.com/generalaction/emdash/issues/1875)

## Scope

Linux credential storage lane from the 2026-05-28 repo-universe fallback cycle. The issue reports that Electron/Chromium can choose the plaintext `basic_text` backend for `safeStorage` on Hyprland, sway, i3, dwm and other non-GNOME/KDE desktops, causing Emdash to reject all secret persistence even when Secret Service works.

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this cycle used fallback: 3 of 6 read-only scouting roles until the agent thread limit was reached, parent live GitHub gates, then 3-role critique.

No upstream PR/comment/ping was made.

## Parent live gates

Live state at `2026-05-28 01:45 UTC`:

- Issue `#1875` is open and unassigned.
- Maintainer/contributor signal is positive: the report makes sense and a PR is welcome.
- Targeted open PR searches for `#1875`, `safeStorage`, `password-store`, `gnome-libsecret`, and `basic_text` found no exact covering PR.
- Repo requires Node `24+` and pnpm `10.28+`; expected local checks are format, lint, typecheck and tests.
- Patch surface appears to be around `src/main/index.ts` and `EncryptedAppSecretsStore`, but no local source checkout/repro was run in this cycle.

## Decision

`next_status: CANDIDATE`

Reason: strong issue write-up, explicit maintainer welcome, no exact PR found, and a narrow security-adjacent but user-facing failure mode. It is not `PR-READY` because current-main fail-before evidence and design choice are missing.

Upstream action count: `0`.

## Runner gate

Promotion requires a secret-free Linux runner repro:

- Run with `XDG_CURRENT_DESKTOP=Hyprland` or equivalent non-GNOME/KDE value.
- Ensure Secret Service owns `org.freedesktop.secrets` on the session bus.
- Verify current Emdash rejects `basic_text` and fails secret persistence without storing real credentials.
- Test the chosen mitigation: either force `--password-store=gnome-libsecret` when Secret Service is available, or improve docs/error text if maintainers prefer no auto-detection.

Do not store private tokens in repo. Use fake secrets or local throwaway credentials only.

## Promotion triggers

- `PR-READY`: runner-backed fail-before, minimal patch, focused test/manual verification, fresh duplicate search, and 3-role critique.
- `COMMENT-FIRST`: if the design choice between auto-detection and documentation/error-only remains unclear after repro.
- `WATCH`: if an exact upstream PR appears or maintainer chooses a different direction.
