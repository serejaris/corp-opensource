# High-churn terminal / runner repo scope, 2026-05-27

Tracker: [#74](https://github.com/serejaris/corp-opensource/issues/74)

Scope: `openai/codex`, `anomalyco/opencode`, `google-gemini/gemini-cli`, `daytonaio/daytona`.

Live check: 2026-05-27 ~22:00 UTC. Upstream не трогал.

## Required skills fallback

Обязательные skills `open-source-bug-scouting` и `open-source-pr-workflow` в этой сессии недоступны как локальные skill files. Fallback: 6-role subagent scouting, parent live GitHub gates через `gh`, затем 3-role critique перед tracker/watch update.

## Parent live gates

| Repo | Live repo signal | Open queue | Process / community gate | Fresh issue / PR signal |
|---|---|---:|---|---|
| `openai/codex` | Apache-2.0, Rust, `86,314` stars / `12,628` forks, pushed `2026-05-27T21:56:45Z`, latest `rust-v0.134.0` (`2026-05-26`) | `gh issue list --limit 1000` capped at 1000 issues; `304` open PRs; API parent count `5264` issues+PRs | Community health `75`; contributing docs gate external code contributions as invitation-only, so external PR is not a default path | Fresh issues `#24823`, `#24815`, `#24806`, `#24796`; active same-hour PRs `#24828/#24826/#24825/#24819/#24816/#24813/#24805/#24801/#24798/#24791/#24776/#24773` cover SDK/app-server/hooks/network/permissions nearby |
| `anomalyco/opencode` | MIT, TypeScript, `166,142` stars / `19,766` forks, pushed `2026-05-27T21:40:59Z`, latest `v1.15.11` (`2026-05-27`) | `gh issue list --limit 1000` capped at 1000 issues; `939` open PRs; API parent count `6219` issues+PRs | Community health `87`; issue-first, linked issue and small focused PR expected; core UI/product work needs design review | Fresh issues `#29633/#29629/#29628/#29624/#29621/#29620/#29619/#29618/#29616`; fresh PRs `#29632/#29631/#29630/#29627/#29625/#29615/#29582/#29565/#29562` create immediate collision pressure |
| `google-gemini/gemini-cli` | Apache-2.0, TypeScript, `104,660` stars / `13,901` forks, pushed `2026-05-27T21:52:32Z`, latest `v0.44.0` (`2026-05-27`) | `gh issue list --limit 1000` capped at 1000 issues; `279` open PRs; API parent count `1477` issues+PRs | Community health `75`; linked issue, Google CLA, `npm run preflight`; `help wanted`/approved issues only, `maintainer only` is no-go | Fresh issues `#27499/#27491/#27475/#27470/#27466`; fresh PRs `#27496/#27474/#27473/#27472/#27467/#27463/#27458/#27453/#27429` cover PTY/core/security/auth adjacency |
| `daytonaio/daytona` | AGPL-3.0, TypeScript, `72,456` stars / `5,594` forks, pushed `2026-05-27T21:41:58Z`, latest `v0.182.0` (`2026-05-26`) | `276` open issues; `130` open PRs; API parent count `406` issues+PRs | Community health `87`; issue-first, DCO signoff, generated docs/API/lint as applicable; security/runtime lanes require process care | Fresh issues `#4842/#4805/#4750/#4740/#4683/#4681/#4680`; fresh PRs `#4839/#4838/#4837/#4833/#4832/#4808` cover computer-use, proxy/runner, SDK timeout and SSH lifecycle nearby |

## 6-subagent synthesis

| Role | Summary | Status |
|---|---|---|
| Repo fit / activity | `anomalyco/opencode` and `openai/codex` have the strongest direct terminal-agent fit, but both are extremely hot queues. `google-gemini/gemini-cli` is structured but label/process-heavy. `daytonaio/daytona` is the best sandbox/runner adjacency. | `WATCH` |
| Issue actionability | Potential later lanes: `daytonaio/daytona#4750`, `google-gemini/gemini-cli#27470`, `openai/codex#24806`, `opencode#29620`; none is promotable without current-main repro and duplicate search. | `WATCH` |
| Duplicate / process | Codex external PR path is invitation-only; opencode has same-day PR collision; Gemini labels and linked issue policy block cold PRs; Daytona likely has `#4750`/`#4805` covered by `#4832`/`#4808`. | `WATCH` |
| Community / contribution | Daytona is most conventionally open, but AGPL/security/runtime caution applies. Gemini requires approved/help-wanted path. Opencode is friendly but guarded. Codex is evidence/comment-only unless invited. | `WATCH` |
| Runner / repro | Gemini has the best secret-free unit surface; Codex has mockable Rust/CLI lanes; opencode often needs Bun/TUI/provider setup; Daytona meaningful infra bugs need dedicated runner/services. | `WATCH` |
| Final synthesis | Promotion order for later: Daytona first, Gemini second, Codex/opencode only narrowly. No upstream action in this slice. | `WATCH` |

## 3-role critique

| Critique role | Result |
|---|---|
| Фактология / дубликаты | Нельзя называть ни один lead `CANDIDATE`: нет current-main repro и полного duplicate/linked-PR gate. Записать collision risks: Codex `#24806/#24796` vs permissions/network/app-server PRs; opencode `#29628` vs `#29562`, `#29620/#29629` vs `#29630`, reasoning cluster vs `#29565`; Gemini `#27499` vs `#27496`, CJK `#27470/#27491` still duplicate-labeled; Daytona `#4750` vs `#4832`, `#4805` vs `#4808`. |
| Process gates | 6-subagent scouting and parent live gates are done. Per-issue gates are not complete. Runner note is recorded; no repro/test was run. Upstream action is process-unsafe now. |
| Actionability | Exactly one final status: `WATCH`. `CANDIDATE` and `COMMENT-FIRST` are premature until one issue gets current-main repro, exact duplicate/PR search, regression card and safe process gate. |

## Runner

Dedicated `corp-opensource-runner` was not used because this cycle stayed at repo/watch scouting and no issue was promoted to repro. Local machine was used only for `gh` live gates and tracker updates.

Future runner gates:

- Daytona: self-hosted sandbox/runner/proxy for `#4805/#4740/#4680`, or SDK/unit slice only after `#4832` resolves.
- Gemini CLI: secret-free Node/Vitest path for CJK/PTY serialization; Windows/ConPTY requires suitable runner/host.
- Codex: Rust/CLI/plugin/MCP mock tests can be secret-free; sandbox/network/app issues need environment matrix.
- Opencode: Bun/TUI/provider mocks can be secret-free; desktop/Windows/provider auth lanes need heavier runner.

## Decision

`next_status: WATCH`.

No upstream PR/comment. Next useful cycle should start with Daytona post-PR-settle gap scan, then Gemini help-wanted/approved issue filtering. Codex and opencode remain watch-only unless there is a narrow maintainer-friendly issue with fresh duplicate gate and runner-backed evidence.
