# AI-native popular repo universe, cycle 31, 2026-05-28

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)
Promoted issue tracker: [#82](https://github.com/serejaris/corp-opensource/issues/82)

## Scope

Fresh live repo-universe pass for coding-agent control planes, terminal session managers and MCP infrastructure repos that appeared in 2026-05-28 searches.

Required `open-source-bug-scouting` is not installed in this environment, so this cycle used the documented fallback: 6 read-only subagents, parent live GitHub gates through `gh`, then internal 3-role critique.

No upstream PR/comment/ping was made.

Tracker update: [#52 comment](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4559754605).

Follow-up update: [upstream follow-up 2026-05-28](upstream-followup-2026-05-28.md) demoted the selected `stablyai/orca#2930` lane from `CANDIDATE` to `WATCH / duplicate-covered` after a later live gate found exact open non-draft upstream PR [stablyai/orca#2948](https://github.com/stablyai/orca/pull/2948). The original cycle remains useful as the promotion record, but no runner or upstream action should proceed while `#2948` is live.

## Parent live gates

Selected repos came from live searches for `agent framework llm`, `mcp server ai agent`, `browser agent llm`, and `coding agent cli`.

| Repo | Live signal, 2026-05-28 UTC | Issue / PR pressure | Process / runner gate |
|---|---|---|---|
| `stablyai/orca` | 3.5k stars, TypeScript, pushed `2026-05-28`, latest `v1.4.30`, MIT | 93 open issues / 133 open PRs. Best lane `#2930` short stream chunks zero-fill remote file reads; `#2931` secondary PTY buffer cap lane. | Node 24 / pnpm 10. PR template requires AI review, security audit, cross-platform notes. High same-hour duplicate velocity. |
| `generalaction/emdash` | 4.7k stars, TypeScript, pushed `2026-05-27`, latest `v1.1.25`, Apache-2.0 | 48 open issues / 74 open PRs. Best later lanes `#1875` Linux safeStorage and `#2110` PTY kill for `setsid()` descendants. | Node 24+ / pnpm 10.28+. `#1875` needs design choice; `#2110` needs repro. |
| `asheshgoplani/agent-deck` | 2.5k stars, Go, pushed `2026-05-27`, latest `v1.9.42`, MIT | 3 open issues / 18 open PRs. `#1212` is runner-friendly web e2e/sidebar lane; `#1205` is Codex send false-negative but maintainer queued fix. | Go/tmux/Make. Low issue volume; prefer obvious bugfixes only after runner proof. |
| `agentgateway/agentgateway` | 2.9k stars, Rust, pushed `2026-05-27`, latest `v1.3.0-alpha.1`, Apache-2.0 | 180 open issues / 52 open PRs. `#1958/#1976/#1977` are covered/adjacent; `#1944` is good-first perf watch. | Rust/Go/UI stack. Infra/security/K8s risk; comment-first for nontrivial behavior. |
| `googleapis/mcp-toolbox` | 15.4k stars, Go, pushed `2026-05-27`, latest `v1.3.0`, Apache-2.0 | 136 open issues / 105 open PRs. `#3288` covered by `#3289`; `#3286` is feature/comment-first and maintainer-routed; `#3278` likely client/OAuth side. | Google CLA, issue-first, tests and integration expectations. |
| `proxysoul/soulforge` | 715 stars, TypeScript/Bun, pushed `2026-05-27`, latest `v2.18.4`, BUSL-1.1 | 0 open issues / 3 open PRs. No selected public bug lane. | BUSL until Apache conversion date; strict governance. Low-priority watch only. |

## 6-subagent synthesis

- Repo fit / freshness: promote `stablyai/orca`, `generalaction/emdash`, `agentgateway/agentgateway`, `googleapis/mcp-toolbox`, and `asheshgoplani/agent-deck` as watch targets. Keep `proxysoul/soulforge` low-priority because license/governance and issue surface are poor for generic OSS work.
- Issue quality: `stablyai/orca#2930` is the strongest lane because it is high-impact data corruption with concrete file, repro and test shape. `orca#2931` is secondary. `emdash#2110` is a lead only.
- Duplicate race: Orca has extreme same-hour issue-to-PR velocity, but exact `#2930` PR was not found. `mcp-toolbox#3288`, `agentgateway#1958`, `orca#2928/#2929`, and `agent-deck#1205` are covered/claimed.
- Process/license: Orca, Emdash, Agent Deck, Agentgateway and MCP Toolbox are process-safe for future contribution with repo-specific gates. Soulforge is selective/no-go for broad scouting due BUSL and strict governance.
- Runner/repro: `agent-deck#1212` is easiest in runner, but `orca#2930` has higher impact and enough source evidence for internal candidate. Runner is required before PR-ready.
- Conservative synthesis: update repo cards and umbrella tracker; no upstream action. Parent source and critique gates promoted `orca#2930` to dedicated internal `CANDIDATE`.

## 3-role critique

- Factology/duplicates: `stablyai/orca#2930` is open, unassigned, no labels, no exact covering PR; source confirms `Buffer.alloc(totalSize)` plus chunk-count-only end validation.
- Process gates: internal `CANDIDATE` is allowed, but upstream comment/PR is blocked until runner repro, final duplicate search and PR-template review/security/cross-platform notes.
- Actionability: `CANDIDATE`; not `COMMENT-FIRST` because the upstream issue already contains the useful technical details; not `PR-READY` because no runner fail-before or patch exists.

## Decision

`next_status: CANDIDATE`

Selected lane: `stablyai/orca#2930`, tracked in [#82](https://github.com/serejaris/corp-opensource/issues/82) and [watch/orca-2930-short-stream-zero-fill.md](orca-2930-short-stream-zero-fill.md).

Upstream action count: `0`.

Runner was not used. Reason: scouting/source-gate cycle only; PR-ready work requires dedicated `corp-opensource-runner` with Node 24 / pnpm 10. CT `216` remains Hermes-only and was not used.

## Next actions

- For `orca#2930`: watch open PR `#2948` merge/close/stall/gap; rerun runner/duplicate gates only if the PR stops covering the lane.
- For `agent-deck#1212`: keep as runner-friendly backup lane.
- For `emdash#1875/#2110`: keep as design/repro leads.
- For `mcp-toolbox#3286`: comment-first only if maintainers explicitly welcome external PR.
- For `agentgateway`: watch only; current MCP/UI/config lanes are mostly covered or process-heavy.
- For `soulforge`: revisit only on a concrete public bug lane.
