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

This is a good CUA target, but not first unless we have Windows/runtime evidence. A local synthetic Rust test may cover parsing, but upstream acceptance likely needs confidence that the marker lands correctly on Windows with DWM/DPI/window offset.

## Decision

`GO with runner`: best non-pydantic CUA candidate. Do not open a PR until we can run or convincingly test the Windows recorder path.
