# AI-native / Paperclip-like scouting, 2026-05-28 cycle 35

Scope: CUA / UI automation / agent-framework repo refresh.

Tracker: [#72](https://github.com/serejaris/corp-opensource/issues/72), umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52).

Tracker comments: [#72 cycle 35](https://github.com/serejaris/corp-opensource/issues/72#issuecomment-4560503554), [#52 heartbeat](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560504225).

Required `open-source-bug-scouting` skill is unavailable in this environment, so this cycle used the documented fallback: parent live GitHub gates, 6 read-only roles in two waves, then a 3-role factology/process/actionability critique.

No upstream PR/comment/ping was made.

Runner was not used: this was a repo-scope `WATCH` refresh. Runner/current-main repro is required before promoting any lane to `CANDIDATE`/`PR-READY` or making upstream claims.

## Result

`next_status: WATCH`

Upstream action count: `0`.

Primary runner target if the dedicated runner becomes available: `web-infra-dev/midscene#2544` (`longPress` capped to 600ms), after a fresh duplicate gate and contract check.

## Live repo gates

| Repo | Live signal | Decision |
|---|---|---|
| `web-infra-dev/midscene` | `13,451` stars, MIT, pushed `2026-05-28`; UI automation / browser-use / phone-use topics. Fresh bugs: `#2544` longPress caps to 600ms with repro repo, `#2539` `/execute` oversized response `RangeError`, `#2549` `photon_rs_bg.wasm` init failure. PR search found no exact live cover, but nearby history exists: `#2387` exposed `aiLongPress`, `#2522/#2252` touch report/output paths, `#1896/#2462` touch photon/packaging. | Best new `WATCH` target. Do not call `#2544` candidate until current-main browser repro proves the contract and duplicate search stays clean. |
| `CursorTouch/Windows-MCP` | `5,758` stars, MIT, pushed `2026-05-26`; Windows MCP/UI automation niche. `#236` has maintainer PR-welcome signal but also an active external branch/work thread; `#82/#99` are coordinate drift cluster; stale PRs `#81/#93` attempted DPI coordinate support. | `WATCH / duplicate-sensitive`; needs Windows privileged/DPI runner and stale-PR applicability check before any upstream action. |
| `bytedance/UI-TARS-desktop` | `35,528` stars, Apache-2.0, pushed `2026-05-18`; CUA/GUI-agent/MCP topics. Fresh issues are macOS Tahoe / topmost / iPhone mirror environment-specific. Open PRs `#1910` scale/type changes and `#1906` security hardening are not exact covers. | High-value passive `WATCH`; no promotion without macOS/device repro evidence. |
| `coasty-ai/open-computer-use` | `663` stars, Apache-2.0, pushed `2026-05-28`; production branch, computer-use agent topics; no open issues or PRs. | Reference `WATCH`; no bug lane yet. |
| `the-open-agent/openagent` | `5,017` stars, Apache-2.0, pushed `2026-05-27`; open `#2303` Windows version endpoint path issue, but `#2281` reporter says fixed in `2.18.1` and `#2282` is assigned/update guidance. | Passive `WATCH`; needs Windows package/binary repro before candidate. |
| `higress-group/himarket` | `1,167` stars, license `Other`, pushed `2026-05-27`; AI capability marketplace / MCP server registry. `#304` HiChat MCP 500 is fresh, but `#278/#279` are assigned and `#289` says resolved; PR `#303/#282` cover upload limits. | Ecosystem reference only; weak direct Paperclip-like fit unless a narrow agent/tool registry validation bug appears. |
| `microsoft/agent-framework` | `10,796` stars, MIT, pushed `2026-05-28`; existing [#68](https://github.com/serejaris/corp-opensource/issues/68) watch. `#6120` is exact-covered by open PR `#6128` (`Fixes #6120`, `ChatClientAgent` + regression test), with light policy/check status only. | Existing exact-subsystem `WATCH`; no new tracker. Re-enter only if `#6128` closes unmerged or lands incomplete. |

## 6-role synthesis

- Repo-fit: `midscene` is the best new CUA/UI automation watch target; `Windows-MCP` is the best secondary Windows-specific watch.
- Duplicate/process risk: `agent-framework#6120` is blocked by exact cover `#6128`; `Windows-MCP#236` has active human work; `Windows-MCP#82/#99` have stale DPI PR history.
- Environment gates: UI-TARS needs macOS/device evidence; Windows-MCP and openagent need Windows runner/package repro; himarket needs a service-stack repro.
- Tracker consistency: no new internal issue is warranted because `#72` already covers Cline/CUA/Playwright MCP repo scope; create a dedicated issue only after runner-backed evidence.
- Final downgrade: `midscene#2544` is promising, but stays `WATCH` rather than `CANDIDATE` until current-main repro proves this is a contract bug rather than expected duration clamping or a legacy API gap.

## 3-role critique

- Factology: avoid overclaiming that `midscene#2544/#2539/#2549` are uncovered. Parent PR search found no exact live cover, but nearby historical PRs require code-path confirmation.
- Process: internal README/repo-scope/watch updates and tracker heartbeat are allowed. Upstream comments would be premature.
- Actionability: next concrete work is a runner-backed `midscene#2544` repro with a minimal browser/web integration test; if that passes and duplicate gate remains clean, open a dedicated internal candidate tracker.

## Unblock events

- `midscene`: `corp-opensource-runner` available with Node/browser deps; current-main repro confirms `longPress >600ms` contract failure; fresh PR search stays clean.
- `Windows-MCP`: Windows runner available; stale DPI PRs and active UAC branch are checked against current main.
- `UI-TARS-desktop`: macOS/device-specific repro environment available.
- `openagent`: latest Windows package/binary repro confirms `#2303`.
- `himarket`: minimal Nacos/HiChat service stack repro confirms `#304` and identifies repo-owned patch surface.
- `agent-framework`: `#6128` closes unmerged or merged fix does not close `#6120`.
