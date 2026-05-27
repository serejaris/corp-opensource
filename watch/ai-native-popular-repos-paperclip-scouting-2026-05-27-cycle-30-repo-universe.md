# AI-native popular repo universe, cycle 30, 2026-05-27

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

## Scope

Fresh live repo-universe pass for popular Paperclip-like / AI-native targets not yet fully represented in repo cards. Required `open-source-bug-scouting` is not installed in this environment, so this cycle used the documented fallback: 6 read-only subagents, parent live gates through `gh`, then internal 3-role critique.

No upstream PR/comment/ping was made.

Tracker update: [#52 comment](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4559629781).

## Parent Live Gates

Live search queries covered `topic:ai-agent`, `agent framework llm`, `coding agent`, and `browser agent llm` repositories. Parent gates then checked six selected repos for activity, issue/PR pressure, licenses, and fresh issue/PR lanes.

| Repo | Live signal, 2026-05-27 23:45 UTC | Issue / PR pressure | Process / runner gate |
|---|---|---|---|
| `browseros-ai/BrowserOS` | 11.1k stars, pushed `2026-05-27T23:37:25Z`, default `main`, root `LICENSE` is AGPL-3.0 | 76 open issues / 40 open PRs. Best lane `#1005` MCP `notifications/tools/list_changed` flood; backup `#1029` Node 18 / undici MCP failure. | Agent/MCP layer is in scope; Chromium-core/full browser build is out of scope without dedicated runner. |
| `can1357/oh-my-pi` | 7.9k stars, pushed `2026-05-27T23:37:24Z`, MIT | 105 open issues / 113 open PRs. `#1460` already has open clean PR `#1461`; `#1459/#1458` are lead-only after duplicate gate. | High duplicate velocity; Bun/Rust/terminal runner needed for real repro. |
| `charmbracelet/crush` | 24.7k stars, pushed `2026-05-27T22:30:41Z`, root `LICENSE.md` is FSL-1.1-MIT | 296 open issues / 159 open PRs. Best lanes `#3011` malformed Authorization header and `#3024` rate-limit retry behavior. | CLA/FSL gate; Go/TUI/provider mock runner needed before comment or PR. |
| `ComposioHQ/agent-orchestrator` | 7.3k stars, pushed `2026-05-24T18:33:25Z`, MIT | 456 open issues / 463 open PRs. `#2063` is covered by open PR `#2064`; best future lane `#2051` Windows AO dashboard/session error. | High churn and Windows runner requirement; repo stays `LEAD / WATCH`. |
| `JetBrains/koog` | 4.2k stars, pushed `2026-05-27T18:56:03Z`, default `develop`, Apache-2.0 | 76 open issues / 73 open PRs. Best lane `#2020` Anthropic enum casing; `#2017` is covered by PRs `#2067/#2071`. | JetBrains/process-heavy; branch gate is `develop`; Gradle/KMP runner needed for repro/test. |
| `alibaba/OpenSandbox` | 10.8k stars, pushed `2026-05-27T10:37:59Z`, Apache-2.0 | 47 open issues / 23 open PRs. Best lane `#949` signed endpoints + `use_server_proxy`; `#946/#934` are runner-heavy follow-ups. | CLA/OSEP/design gate; Docker/K8s/runtime runner needed for real proof. |

## 6-subagent Synthesis

- `browseros-ai/BrowserOS`: promote as high-fit browser-agent/MCP repo card. `#1005` is the best issue-level candidate for a future cycle; no Chromium-core work without dedicated runner.
- `can1357/oh-my-pi`: keep as `WATCH / LEAD-SCOUT`. It is highly active and relevant, but fresh issues are quickly claimed or PR-covered.
- `charmbracelet/crush`: promote as terminal coding-agent watch. `#3011` is the cleanest candidate; `#3024` is comment/repro-first because maintainer/reporter discussion is already active.
- `ComposioHQ/agent-orchestrator`: keep as multi-agent orchestrator lead. `#2051` is useful but Windows-specific; covered lanes `#2063/#2057/#2008` should not be reopened.
- `JetBrains/koog`: promote as JVM agent-framework watch. `#2020` is the first candidate lane; MCP/tool-call lanes are already duplicate-heavy.
- `alibaba/OpenSandbox`: promote as secure sandbox/runtime watch near E2B/Daytona, but first actionable lane `#949` needs maintainer/design direction before any patch.

## 3-role Critique

- Factology/duplicates: approved with downgrades. Do not mark `oh-my-pi#1460`, `agent-orchestrator#2063`, or `koog#2017` as candidates because they have active covering PRs. Keep `OpenSandbox#949` at `COMMENT-FIRST` ceiling because of API/design risk.
- Process/license/runner: approved for internal README/watch/#52 update only. No upstream comment, PR, or runner is needed in this repo-level cycle. Record license/process gates: BrowserOS AGPL, Crush FSL/CLA, Koog `develop` branch, OpenSandbox CLA/OSEP, pi `lgtm` gate, agent-orchestrator Windows runner.
- Actionability: approved. Promote repo cards for BrowserOS, OpenSandbox, Koog, and Crush; keep agent-orchestrator and oh-my-pi as lower-priority lead/watch; no full-scope `NO-GO`.

## Decision

`next_status: WATCH / repo-universe update`

Upstream action count: `0`.

Promoted repo-card/watch targets:

- `browseros-ai/BrowserOS`: browser-agent/MCP layer; next issue-level cycle should start with `#1005`.
- `alibaba/OpenSandbox`: secure sandbox/runtime layer; next issue-level cycle should start with `#949`.
- `JetBrains/koog`: JVM agent framework; next issue-level cycle should start with `#2020`.
- `charmbracelet/crush`: terminal coding-agent; next issue-level cycle should start with `#3011` or `#3024`.

Lower-priority leads:

- `ComposioHQ/agent-orchestrator`: strong multi-agent coding orchestrator, but best lane `#2051` needs Windows runner.
- `can1357/oh-my-pi`: strong terminal-agent watch, but current fresh lanes are highly duplicate-prone.
- `earendil-works/pi`: canonical popular project; no PR without maintainer `lgtm`.

Runner was not used. Reason: this was repo-level scouting, not an issue-level repro or PR-ready patch cycle. Future issue-level cycles must use `corp-opensource-runner` or document why it is unavailable; CT `216` remains Hermes-only.
