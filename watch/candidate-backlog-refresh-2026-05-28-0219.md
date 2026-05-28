# Candidate backlog refresh, 2026-05-28 02:19 UTC

Trackers: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52), Probe [#80](https://github.com/serejaris/corp-opensource/issues/80), BrowserOS [#81](https://github.com/serejaris/corp-opensource/issues/81), Emdash [#83](https://github.com/serejaris/corp-opensource/issues/83), Serena [#84](https://github.com/serejaris/corp-opensource/issues/84), Mastra [#58](https://github.com/serejaris/corp-opensource/issues/58), AG-UI [#56](https://github.com/serejaris/corp-opensource/issues/56).

Bounded refresh over six active candidate/watch lanes:

- `oraios/serena#1519`
- `generalaction/emdash#1875`
- `browseros-ai/BrowserOS#1005`
- `probelabs/probe#568`
- `mastra-ai/mastra#17118`
- `ag-ui-protocol/ag-ui#1635`

The required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this cycle used documented fallback: parent live GitHub gates, 6 read-only scouting roles in two waves, then 3-role critique before tracker/watch updates.

Upstream action count: `0`.

No upstream comment, PR, ping, rerun, or rebase was made.

## Parent Live Gates

Live time: `2026-05-28 02:10-02:19 UTC`.

Runner/tooling gate:

- `corp-server` does not resolve from this environment.
- `corp-opensource-runner` is not provisioned per [#10](https://github.com/serejaris/corp-opensource/issues/10).
- Local toolchain: Node `v20.19.2`; no `pnpm`, no `bun`, no Docker, no `rustc/cargo`.
- Therefore no lane can move to `PR-READY` in this cycle.

| Lane | Live result | Decision |
|---|---|---|
| `probelabs/probe#568` | Open, labels `bug` + `external`, no assignee. Upstream `probelabs` / Visor comment confirms legitimate bug shape and asks for exact CLI/API/env details. Open PRs `#561/#264` do not cover symbols JSON stdout pollution. | Primary `CANDIDATE` and first runner target. |
| `oraios/serena#1519` | Open, no labels/assignee/comments. No open PR cover found for dashboard 403 / host-port mismatch; old Dockerfile PR `#1362` is not a cover. Source check still maps to `src/serena/dashboard.py` Host validation. | Reserve `CANDIDATE`; Docker/port-mapping repro required. |
| `browseros-ai/BrowserOS#1005` | Open, no labels/assignee. Reporter's 2026-05-26 update gives current root-cause evidence: full tool re-registration on every MCP tool call/focus/connect. No exact open PR found; MCP-adjacent PRs do not cover the flood. | Reserve `CANDIDATE`; AGPL/CLA and MCP lifecycle gates apply. |
| `generalaction/emdash#1875` | Open, no labels/assignee. Maintainer/contributor says PR welcome. No open cover found, but closed exact/unmerged PR history `#1907-#1911` is mandatory prior art. | `WATCH / CANDIDATE-risk`; not primary until closed PR history is fully incorporated. |
| `mastra-ai/mastra#17118` | Open, labels `bug`, `trio-tb`, `trio-wp`, `impact:medium`, `effort:medium`, `mastracode`. Maintainer/team asks for minimal repro. No exact PR found, but Mastracode/TUI queue is dense and `#17054` is relevant partial precedent for ask_user wrapping. | `COMMENT-FIRST / WATCH` after MRE; not primary. |
| `ag-ui-protocol/ag-ui#1635` | Open, no labels/assignee/comments. No exact open PR found for missing-payload chunk abort. Broad stale PR `#1253` strongly overlaps the Mastra adapter/chunk mapping surface and is `CONFLICTING` / `CHANGES_REQUESTED`. | `WATCH`; no PR without evidence and explicit positioning against `#1253`. |

## 6-Role Synthesis

- Repo/activity: `BrowserOS#1005` and `Serena#1519` remain the strongest strategic Paperclip-like candidates; `Probe#568` is the cleanest small-surface candidate; `Emdash#1875` has strong maintainer signal but risky closed PR history.
- Duplicate/PR pressure: no exact open cover for `Probe#568`, `Serena#1519`, `BrowserOS#1005`, `Emdash#1875`, or `Mastra#17118`; `AG-UI#1635` is blocked by broad adjacent PR `#1253`.
- Process: `BrowserOS` has AGPL/CLA friction; `Mastra` wants MRE and has dense PR queue; `Probe`, `Serena`, and `Emdash` are more direct once evidence exists.
- Runner feasibility: `Probe#568` is now the practical first runner target because it is Rust/CLI, secret-free, and does not require Docker, browser, Bun, Electron, or native desktop services. `Serena#1519` needs Docker/port mapping; `BrowserOS#1005` needs Bun/MCP runtime; `Emdash#1875` needs Linux Secret Service/Electron-sensitive evidence.
- Source plausibility: `Probe#568` still needs exact path proof. `src/extract/symbols.rs::handle_symbols` looks JSON-clean; suspicious `Pattern:` / `Path:` output is in `src/main.rs::handle_search`, so the runner job must capture the exact CLI/API path before any patch.

## 3-Role Critique

- Factology/duplicates: keep `Probe#568` as primary, but do not overclaim root cause. Keep `Serena#1519` and `BrowserOS#1005` as reserve candidates. Mark `Emdash#1875` as prior-art gated, `Mastra#17118` as MRE/process gated, and `AG-UI#1635` as adjacent-overlap watch.
- Process gates: final cycle status can be `CANDIDATE` because several uncovered internal candidates remain. `PR-READY`, `PR-OPEN`, and upstream comment are blocked by missing runner-backed evidence.
- Actionability: create no new tracker issue. Update umbrella `#52` and primary Probe tracker `#80`; avoid noisy heartbeat comments on unchanged reserve lanes.

## Decision

`next_status: CANDIDATE`

Primary next runner target: `probelabs/probe#568`.

Reserve runner targets: `oraios/serena#1519`, then `browseros-ai/BrowserOS#1005`.

Upstream action count: `0`.

Runner action count: `0`.

## Probe #568 Runner Card

Goal: prove exact fail-before path before deciding between `COMMENT-FIRST` and `PR-READY`.

Capture minimum:

- repo SHA and release/current-main reference;
- exact CLI command and/or Node SDK call;
- input fixture;
- raw stdout;
- raw stderr;
- exit code;
- `python -m json.tool` result for stdout;
- Node wrapper parse result if applicable;
- environment variables and verbose/debug flags.

Candidate commands:

```bash
git clone https://github.com/probelabs/probe
cd probe
cargo test
cargo run -- symbols --format json <fixture-file> > /tmp/symbols.stdout 2> /tmp/symbols.stderr
python3 -m json.tool /tmp/symbols.stdout
```

`PR-READY` only if current `main` or `v0.6.0-rc319` proves invalid JSON on stdout for `symbols --format json` or the npm `symbols()` wrapper, and the patch surface is narrowed to the exact output path. The regression must prove JSON stdout stays strict and logs/progress go to stderr or are suppressed for JSON mode.
