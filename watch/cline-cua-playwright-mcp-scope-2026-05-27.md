# Cline / CUA / Playwright MCP scope watch, 2026-05-27

Tracker: [#72](https://github.com/serejaris/corp-opensource/issues/72)
Umbrella: [#52](https://github.com/serejaris/corp-opensource/issues/52)
Decision time: 2026-05-27 22:06 UTC
Final `next_status`: `WATCH`

## Scope

Bounded repo-scope recheck for the remaining Tier 1A coding-agent / computer-use / browser-MCP slice.

| Repo | Fit | Live gate |
|---|---|---|
| `cline/cline` | Coding-agent IDE/CLI product with provider routing, MCP/connectors, tool execution, plugin sandbox and VS Code/webview surface. | Apache-2.0; default `main`; `62417` stars / `6550` forks; `494` open issues / `470` open PRs; latest release `cli-v3.0.14` published `2026-05-27T19:11:12Z`; pushed `2026-05-27T21:14:52Z`. |
| `trycua/cua` | Computer-use agent infra: desktop drivers, screenshot/click coordinate contracts, trajectory recorder, MCP driver and OS automation. | MIT; default `main`; `17143` stars / `1088` forks; `128` open issues / `209` open PRs; latest release `cua-driver-v0.2.0` published `2026-05-17T10:06:17Z`; pushed `2026-05-27T17:21:32Z`. |
| `microsoft/playwright-mcp` | Canonical browser automation MCP server; strong navigation/session/process/artifact lifecycle surface with small backlog. | Apache-2.0; default `main`; `33108` stars / `2718` forks; `6` open issues / `4` open PRs; latest release `v0.0.75` published `2026-05-07T23:08:37Z`; pushed `2026-05-23T14:41:54Z`. |

Required skills note: local skills `open-source-bug-scouting` / `open-source-pr-workflow` are not installed as callable skills in this session, so this cycle used the documented fallback: parent GitHub live gates plus 6-subagent scouting. No upstream comment/PR was attempted.

## Parent live gates

### cline/cline

- Community profile: health `100`, Apache-2.0, CONTRIBUTING, Code of Conduct, issue template, PR template and README are present.
- Test/repro gate: VS Code package version `3.85.0`; extension/CLI repo with unit, integration, e2e and webview surfaces. Provider request-shaping bugs can be mocked; VS Code/plugin/hook paths need runner or local extension harness.
- Active duplicate/process clusters:
  - `#11086` GLM/OpenAI-compatible `reasoning: {exclude: true}` is covered by our open PR `#11087` (`mergeStateStatus=BLOCKED`, waiting review).
  - `#11065` plugin sandbox timeout is covered by `#11084`.
  - `#11061` Discord adapter is covered by `#11077`.
  - `#11026` keybinding tooltip is covered by `#11067`.
  - `#11034` SAP AI Core migration is covered by `#11075`.
- Process gate: Cline is strategically strong but saturated; do not start another Cline PR while current owned PR and adjacent maintainer PRs are active unless there is a fresh small regression with linked issue, current-main repro and clean PR search.

### trycua/cua

- Community profile: health `50`, MIT in pyproject, CONTRIBUTING and README are present; no PR template/Code of Conduct in community profile.
- Test/repro gate: Python `>=3.12,<3.14`, `uv` workspace, pytest under `libs/*/tests`, ruff/black/isort/mypy/pre-commit. Real driver issues need macOS/Windows/Linux display/backend evidence, not just Linux unit tests.
- Active duplicate/process clusters:
  - `#1739` macOS Retina/AVF screenshot-click coordinate mismatch is the best future lead; no direct PR found in this pass, but it is near `#1592/#1593/#1691`.
  - `#1725` Windows element-index `click.png` is covered by open PR `#1737`; our prior source-audit comment already exists.
  - `#1722` macOS bash 3.2 uninstall parse is covered by `#1723`.
  - `#1712` launch_app preflight is covered by `#1719`.
  - `#1600` minimized UWP capture is covered by `#1736`.
  - `#1605` printable keypress layout issue is covered by `#1610/#1660`.
  - `#1537/#1564` heavy webview/window-state lanes overlap `#1609`.
- Process gate: CUA is externally approachable, but platform evidence is the gate. `#1739` cannot move beyond watch without macOS Retina/AVF current-main repro and coordinate-contract analysis.

### microsoft/playwright-mcp

- Community profile: health `75`, Apache-2.0, CONTRIBUTING, Code of Conduct, issue template, PR template and README are present.
- Test/repro gate: Node `>=18`; scripts include `playwright test`, browser-specific projects, and `MCP_IN_DOCKER=1` Docker test mode. No private token needed, but browser/CDP/process and long-run evidence need a stable runner.
- Active duplicate/process clusters:
  - `#1635` `browser_navigate_back` timeout is already comment-first from us and waits maintainer direction because MCP core contribution path may belong in `microsoft/playwright`.
  - `#1636` long-running Chromium memory leak is high-impact but needs controlled memory/process measurements and separation from orphan-process issues.
  - `#1634` orphan Chrome process trees says "with upstream PR"; avoid racing without exact upstream boundary check.
  - `#1629` artifact TTL cleanup is assigned.
- Process gate: Microsoft contribution policy is approval/assignment-first; comments are useful only with new repro, narrowing or duplicate evidence. No new upstream contact now.

## 6-subagent synthesis

| Role | Result |
|---|---|
| Repo fit/activity | Ranking: `microsoft/playwright-mcp` best clean immediate browser/MCP fit, `cline/cline` maximum coding-agent product fit but overheated, `trycua/cua` deep computer-use fit with platform risk. Recommended `WATCH`. |
| Issue quality/actionability | `trycua/cua#1739` is the best new possible lead after macOS repro; `playwright-mcp#1635` is already comment-first; Cline's best lane is already PR-open. Recommended `WATCH`. |
| Duplicate/PR risk | Cline and CUA current obvious lanes are covered by active PR clusters; Playwright MCP backlog is small but approval-gated and `#1634/#1636` overlap lifecycle boundaries. Recommended `WATCH`. |
| Process/community gates | Cline requires linked/small bug discipline, CUA requires repro/env logs, Playwright MCP requires approval/assignment and possibly Playwright monorepo routing. Recommended `WATCH`. |
| Runner/repro feasibility | Playwright MCP is best secret-free runner target; CUA needs OS-specific macOS/Windows evidence; Cline provider lanes can be unit-tested but current candidate is already open. Recommended `WATCH`. |
| Final synthesis | No safe new upstream action; next trigger is Playwright MCP maintainer reply or CUA `#1739` platform repro. Recommended `WATCH`. |

## Follow-up lanes

### microsoft/playwright-mcp

- Keep `#1635` as best immediate browser/MCP follow-up; wait for maintainer direction before PR.
- If runner time is available, prepare secret-free repro for `#1635` without commenting: current main SHA, Node/browser versions, exact MCP command, CDP/non-CDP comparison, and targeted Playwright test idea.
- Treat `#1636` separately; it needs long-run memory/process evidence and separation from `#1634`.

### trycua/cua

- Promote `#1739` only after macOS Retina/AVF current-main repro, duplicate search over `#1592/#1593/#1691`, and coordinate-contract card.
- Keep `#1725/#1737` watch-only; do not open a competing PR/comment unless maintainers ask, `#1737` closes without merge, or Windows smoke still fails.
- Avoid Windows/macOS native fixes without a platform runner/host matrix.

### cline/cline

- Wait for `#11087` review/merge/close before considering another Cline implementation.
- Post-merge gap scan provider/OpenAI-compatible lanes only if `#11086` remains broken or a distinct unsupported-field regression appears.
- Avoid product/QA/Kanban/hook issues without local VS Code repro and maintainer signal.

## 3-role critique

No upstream comment/PR is being sent, so this is an internal critique:

- Factology/duplicates: do not claim CUA `#1739` is duplicate-clean beyond this bounded pass; it is near screenshot/coordinate PRs. Do not treat Playwright MCP `#1635` as PR-ready; it is approval-gated and already has our comment. Do not treat Cline as open for new PR work while `#11087` is active.
- Process gates: Playwright MCP requires maintainer approval/assignment, CUA requires platform-specific evidence, and Cline feature/core work requires discussion or approved issue.
- Actionability: exactly one status is selected: `WATCH`. Unblock event is maintainer direction on `playwright-mcp#1635`, macOS Retina/AVF repro for `trycua#1739`, or completion of the current Cline PR cluster.

## Runner

Runner was not used in this cycle. Reason: repo/issue/PR scouting only; no issue reached `CANDIDATE` or status-changing `COMMENT-FIRST`. Dedicated `corp-opensource-runner` is useful for Playwright MCP browser/CDP/process evidence, but CUA likely also needs OS-specific macOS/Windows host capability. CT `216` remains Hermes-specific and was not used.

## Decision

Keep all three repos in scope, but do not open upstream comments or PRs now.

- `microsoft/playwright-mcp`: best immediate follow-up after maintainer direction on `#1635`.
- `trycua/cua`: best computer-use future lead is `#1739`, blocked on macOS Retina/AVF repro.
- `cline/cline`: high-value but saturated; monitor `#11087` and only do post-review gap scans.
