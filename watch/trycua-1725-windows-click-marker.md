# CUA Windows Element-index Click Marker Watch

## Upstream

- Issue: https://github.com/trycua/cua/issues/1725
- Internal issue: https://github.com/serejaris/corp-opensource/issues/15
- Repo: `trycua/cua`
- Status: open bug
- Internal priority: candidate, but requires Windows/Rust recorder proof.

## Why In Scope

This is a direct CUA / computer-use harness reliability bug. Recorded trajectories miss `click.png` markers for Windows `element_index` clicks, which weakens replay/debugging evidence for agent UI actions.

## Bug Signal

The issue reports that Windows Rust `cua-driver` writes `click.png` only for pixel clicks with explicit `x/y`. Normal UIA `element_index` clicks carry coordinates in `result_summary`, so the recorder skips the marker branch.

Likely patch surface from the issue:

- `libs/cua-driver/rust/crates/cua-driver-core/src/recording.rs`
- possible reuse / extraction from `recording_loader.rs` `parse_screen_from_summary`
- coordinate conversion from screen-absolute to window-local before drawing marker

## Duplicate Check

Checked on 2026-05-27 01:38 -03:

```text
gh search prs --repo trycua/cua "1725 OR click.png OR element_index OR recorder" --state open
[]
```

Nearby no-go duplicates from subagent duplicate sweep:

- `trycua/cua#1722` already has PR `#1723`.
- `trycua/cua#1501` already has PR `#1502`.

Rechecked on 2026-05-27 02:05 -03:

```text
gh issue view 1725 --repo trycua/cua --json state,title,comments,labels,url
OPEN, no comments, label bug

gh search prs --repo trycua/cua "1725 OR click.png OR element_index OR recorder" --state open
[]
```

Follow-up comments:

- Internal tracker: https://github.com/serejaris/corp-opensource/issues/15#issuecomment-4551471214
- Upstream source-audit note: https://github.com/trycua/cua/issues/1725#issuecomment-4551472165

## Current Main Source Audit

Current upstream `main` appears to already include the code path this issue asked for:

- `libs/cua-driver/rust/crates/cua-driver-core/src/recording.rs:66` defines an `ELEMENT_BOUNDS_FN` callback for resolving `element_index` to a window-local click point.
- `libs/cua-driver/rust/crates/cua-driver-core/src/recording.rs:365` falls click-family tools back from explicit `x/y` to `(window_id, pid, element_index, ELEMENT_BOUNDS_FN)` and writes `click_point` into `action.json`.
- `libs/cua-driver/rust/crates/cua-driver-core/src/recording.rs:410` writes `click.png` when `click_point` exists.
- `libs/cua-driver/rust/crates/platform-windows/src/recording_hooks.rs:42` resolves Windows `element_index` via `ElementCache`, then converts screen coordinates to window-local coordinates by subtracting `WindowInfo.x/y`.
- `libs/cua-driver/rust/crates/cua-driver/src/main.rs:555` and `:620` register the Windows element-bounds callback in both registry startup paths.
- `libs/cua-driver/rust/crates/platform-windows/src/tools/impl_.rs:5122` shares the Windows `ElementCache` with the recording hook layer.

The relevant commits in this path are:

```text
b598be62 feat(cua-driver-rs)(recording): native ScreenCaptureKit on macOS + app_state.json/click.png regressions (#1720)
497eab52 feat(cua-driver-rs)(recording): rename set_recording→start/stop + cross-platform video + zoom-on-click renderer + rename crate to cua-driver-core (#1718)
```

Local Rust validation was not run because this machine does not have `cargo` installed:

```text
cargo test -p cua-driver-core recording
zsh:1: command not found: cargo
```

## Repro / Test Plan

Manual repro from issue:

```bash
find <recording-dir> -name click.png
```

after a Windows Calculator recording driven by `element_index` clicks.

Expected regression shape:

- unit-test parser / click-point recovery from `result_summary`;
- preserve existing pixel-click behavior;
- prove screen-to-window-local coordinate conversion or avoid claiming marker accuracy until Windows evidence exists.

## PR Readiness

This is no longer a clean PR target without first proving current `main` still fails. Source audit suggests `#1718` / `#1720` may have already covered the missing path, while issue `#1725` remained open.

## Decision

`WATCH / likely fixed on main pending Windows smoke`: do not open a PR now. Next useful action is a Windows recording smoke or a precise upstream comment saying current `main` appears to cover the issue and asking whether Windows verification is still needed.
