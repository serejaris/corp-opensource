# CUA Windows Element-index Click Marker Watch

## Upstream

- Issue: https://github.com/trycua/cua/issues/1725
- Internal issue: https://github.com/serejaris/corp-opensource/issues/15
- Repo: `trycua/cua`
- Status: open bug with competing PR
- Internal priority: watch; do not open a competing PR while `#1737` is pending review.

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

Rechecked on 2026-05-27 17:15 UTC:

```text
gh issue view 1725 --repo trycua/cua --json state,assignees,labels,comments,updatedAt,url
OPEN, no assignee, label bug

gh pr view 1737 --repo trycua/cua --json state,isDraft,mergeable,reviewDecision,mergeStateStatus,statusCheckRollup,files,updatedAt,url,body,title
OPEN, non-draft, MERGEABLE, REVIEW_REQUIRED, BLOCKED
body: Closes #1725
files:
- libs/cua-driver/rust/crates/cua-driver-core/src/recording.rs
- libs/cua-driver/rust/crates/cua-driver-core/src/recording_loader.rs
- libs/cua-driver/rust/crates/cua-driver/src/main.rs
- libs/cua-driver/rust/crates/platform-windows/src/recording_hooks.rs
checks/status: CodeRabbit success; Vercel failure is team authorization, not a code test failure
```

`#1737` claims the exact issue contract: `element_index` clicks should produce `click.png` in the Windows `cua-driver-rs` trajectory recorder. It overlaps the same recorder/Windows hook surface and adds test-oriented changes per the PR summary. This makes a fresh PR from us a duplicate-risk action.

Important caveat: this is not verified closure. `#1737` is still open, review-required, merge-state blocked, and there is no independent Windows recording smoke from this cycle.

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

This is not a clean PR target. A competing PR now explicitly closes `#1725` and covers the same recorder/Windows path. Do not open a competing PR unless `#1737` is closed without merge, maintainers ask for an alternate fix, or Windows smoke shows the PR/current main still fails the marker contract.

## Decision

`next_status: WATCH`

Reason: competing PR `trycua/cua#1737` is open and pending maintainer review / Windows smoke. No upstream PR or comment from us in this cycle.

Unblock events:

- `#1737` merged or closed;
- maintainer asks for validation or alternate patch;
- Windows recording smoke on `#1737` or current main proves `element_index` clicks still miss `click.png`.

## Cycle 2026-05-27 17:15 UTC

### 6-subagent synthesis

- Repo-fit / PR-readiness: active PRs mostly remain watch-only; no code follow-up on our PRs. `goose#9447`, `opencode#29530`, and `CopilotKit#5035` are now live `MERGEABLE`, but blocked by review/rerun/team auth gates.
- Bug-signal: no new maintainer/reporter signal on Continue, MCP TypeScript, pydantic-ai, OpenHands, or google/adk candidates that would justify a new comment or PR.
- Repro-path: `trycua#1725` still needs Windows smoke for final proof; Linux runner is not sufficient for the real acceptance path.
- Patchability: `trycua#1725` would be patchable, but `#1737` now covers the same path, so patchability does not imply PR-readiness.
- Duplicate-race: `#1737` is the decisive duplicate/competing PR for this candidate.
- PR-readiness: no fresh PR; watch `#1737` review/merge and avoid duplicate work.

### Parent live gates

- Issue state: `trycua/cua#1725` open, label `bug`, no assignee.
- Linked/timeline PR: `trycua/cua#1737` open from 2026-05-27, body says `Closes #1725`.
- PR state: non-draft, `mergeable=MERGEABLE`, `reviewDecision=REVIEW_REQUIRED`, `mergeStateStatus=BLOCKED`.
- Checks/status: CodeRabbit success; Vercel status failure requires Cua team authorization.
- Files touched: recorder core, recording loader, driver registry wiring, Windows recording hooks.
- Regression card: still requires Windows smoke (`find <recording-dir> -name click.png`) before claiming fixed.
- Runner gate: no dedicated Windows/macOS runner used in this cycle; acceptable because status is `WATCH`, not `PR-READY`.

### 3-subagent critique

- Fact-check: do not say fixed or duplicate-covered; correct wording is competing PR pending review. Windows smoke is not proven.
- Process-gate: order satisfied: 6-subagent scouting, parent live gates, 3-subagent critique, then tracker/watch update. Keep `next_status` exactly `WATCH`.
- Actionability: no upstream comment. The actionable unblock event is `#1737` merge/close, maintainer request, or Windows smoke result.
