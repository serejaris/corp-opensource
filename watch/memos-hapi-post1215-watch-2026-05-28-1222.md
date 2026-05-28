# MemOS / hapi post-12:15 watch, 2026-05-28 12:22 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Обязательные skills `open-source-bug-scouting` / `open-source-pr-workflow` в этом окружении всё ещё не установлены. Цикл выполнен через documented fallback: 6 read-only subagent ролей, parent live gates и internal critique.

Upstream comment, PR, ping, rerun или runner action не выполнялись.

`next_status: WATCH`

Fresh repo-scope delta:

- `MemTensor/MemOS` = repo-scope `WATCH`.
- `tiann/hapi` = repo-scope `WATCH`.

Upstream action count: `0`.

Runner action count: `0`.

## Parent live gates

Runner gate остаётся закрытым через [#10](https://github.com/serejaris/corp-opensource/issues/10): dedicated `corp-opensource-runner` не provisioned. CT `216` остаётся Hermes-only и не использовался для general repro.

| Lane | Live result | Decision |
|---|---|---|
| `MemTensor/MemOS` | Новый repo-scope сигнал. На момент live gate: public, non-archived, Apache-2.0, `9,433` stars, pushed `2026-05-28 10:17:59 UTC`, updated `2026-05-28 12:18:25 UTC`. Repo metadata: self-evolving memory OS for LLM/AI agents with persistent memory, hybrid retrieval, skill reuse and MCP. Topics include `memory`, `long-term-memory`, `mcp`, `skills`, `openclaw`, `claude`, `hermes`, `multi-agent`, `self-hosted`. Local README/watch search found no prior `MemTensor/MemOS` entry. | Repo-scope `WATCH`; high-fit memory/context surface, but no issue-level candidate selected. |
| `MemOS#1829/#1815/#1808/#1798/#1797/#1787` | Active open issue surface is memory/plugin/bridge-heavy. Several issues are labeled `ai-task`, `pending`, `plugin`, or broad runtime regressions. Open PR queue is very busy and includes many same-day `Memtensor-AI` fix PRs plus bridge/plugin PRs such as `#1817/#1813/#1799`. | `WATCH`; high churn and likely owner/AutoDev queue. Future pass must pick one bounded plugin/memory bug and repeat duplicate/source/repro gates. |
| `tiann/hapi` | Новый repo-scope сигнал. On live gate: public, non-archived, AGPL-3.0, `4,123` stars, pushed `2026-05-28 07:02:20 UTC`, updated `2026-05-28 10:16:48 UTC`. Repo metadata: app for Claude Code, Codex, Gemini CLI and OpenCode remote/vibe coding. Topics include `claude-code`, `codex`, `gemini-cli`, `opencode`, `remote-control`. Local README/watch search found no prior `tiann/hapi` entry. | Repo-scope `WATCH`; high-fit mobile/remote coding-agent control surface, but no PR-ready lane. |
| `hapi#724/#721/#718/#716/#711/#687/#680/#676/#671` | Open issues include AI policy docs, storage quota, model picker, Gemini remote recovery, ACP plan rendering, voice formatter, OpenCode errors and slash commands. Some narrow surfaces are already PR-covered: `#716` has `#717`, quota/router work has `#722`, and PR queue includes many Codex/OpenCode/voice/web surfaces. | `WATCH`; future pass should choose one bounded web/agent-runtime bug after source checkout and duplicate PR search. |
| `superset-sh/superset#4974` | Fresh post-cutoff OSC 8/custom URL status-line bug was created `2026-05-28 12:16:47 UTC` and closed by `12:20:18 UTC`; old PR `#3257` already overlaps OSC 8 terminal hyperlink handling and is open/dirty. | `NO-GO`; closed issue plus overlapping PR. |
| `anomalyco/opencode#29734` | Fresh post-cutoff issue is assigned to `nexxeln` and duplicate bot links `#25344/#20269/#26181`. | `NO-GO`; assigned plus duplicate-clustered. |
| `nexu-io/open-design#3202/#3208`, `vercel/ai#15670/#15669`, `agent-of-empires#1560/#1571`, `cmux#4938/#4939` | All remain occupied or PR-covered after cutoff. | `WATCH/NO-GO`; do not race. |

Carry-forward backlog stayed open but did not transition to `COMMENT-FIRST` or `PR-READY`: `probe#568`, `claude-peers-mcp#64`, `claude-peers-mcp#65`, `google-gemini/gemini-cli#27503`, `vercel/ai#15652/#15668`, `simular-ai/Agent-S#195`, `browseros-ai/BrowserOS#1005`, `generalaction/emdash#1875`, `topoteretes/cognee#2907`, `agentmemory#703-#708`, `OpenViking#2256`, `cmux`, `open-design`, `clawd-on-desk`, and `agent-of-empires`.

## 6-role synthesis

- Fresh discovery selected `MemTensor/MemOS` as a strong new memory/context repo-scope `WATCH`, and surfaced `superset#4974` only as a duplicate-risk `LEAD`; parent gate then found `#4974` already closed.
- Active backlog refresh found one adjacent repo-local signal, `claude-peers-mcp#65`, but it does not change `#64`: bug 1 overlaps open PRs `#60/#63`, bug 2 is external Claude Code, and bug 3 needs Windows evidence.
- Duplicate/race guard kept fresh issue-looking lanes as `NO-GO` or `WATCH`: `opencode#29734` assigned/duplicate-clustered, `vercel#15670` exact-covered by `#15669`, `open-design#3202` exact-covered by `#3208`, `agent-of-empires` active bug slices covered by `#1560/#1571`.
- Process gates confirmed no lane may move to `COMMENT-FIRST` or `PR-READY`: runner unavailable, many repos are assignment/owner-queue gated, and fresh repo-scope signals lack source/repro evidence.
- Actionability kept no-runner source-first order as `claude-peers-mcp#64`, `Agent-S#195`, `vercel/ai#15652`, `gemini-cli#27503`; true runner order remains `probe#568`, `claude-peers-mcp#64`, `gemini-cli#27503`, `vercel/ai#15652`, `Agent-S#195`.
- Scope/materiality role returned `NO-RECORD` for already-covered/low-signal repos, but parent live gates found two locally absent high-fit repo-scope records: `MemTensor/MemOS` and `tiann/hapi`.

## Critique

- Factology/duplicates: record both repos only as repo-scope `WATCH`. Star counts, topics, license and push/update times are live snapshots. Neither repo has current-main source checkout, full issue-level duplicate proof, or fail-before repro in this cycle.
- Process gates: internal watch/tracker updates are allowed. Upstream comment/PR remains blocked by missing bounded issue selection, runner/source evidence, contribution-process pass and pre-action critique for a concrete patch.
- Actionability: next useful work is either runner-backed `probe#568` when the runner exists, or a small source-first checkout for `claude-peers-mcp#64`. For `MemOS` and `hapi`, future passes must first select exactly one narrow bug lane.

## Next

Keep `MemTensor/MemOS` and `tiann/hapi` on repo-scope watch. Do not open upstream comments or PRs from this cycle.
