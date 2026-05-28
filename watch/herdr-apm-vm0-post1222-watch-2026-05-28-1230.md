# Herdr / APM / vm0 post-12:22 watch, 2026-05-28 12:30 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Обязательные skills `open-source-bug-scouting` / `open-source-pr-workflow` в этом окружении всё ещё не установлены. Цикл выполнен через documented fallback: 6 read-only subagent ролей, parent live gates и duplicate/process synthesis.

Upstream comment, PR, ping, rerun или runner action не выполнялись.

`next_status: WATCH`

Fresh repo-scope delta:

- `ogulcancelik/herdr` = repo-scope `WATCH`.
- `microsoft/apm` = repo-scope `WATCH`.
- `vm0-ai/vm0` = repo-scope `WATCH`.

Upstream action count: `0`.

Runner action count: `0`.

## Parent live gates

Runner gate остаётся закрытым через [#10](https://github.com/serejaris/corp-opensource/issues/10): dedicated `corp-opensource-runner` не provisioned. CT `216` остаётся Hermes-only и не использовался для general repro.

| Lane | Live result | Decision |
|---|---|---|
| `ogulcancelik/herdr` | Новый repo-scope сигнал. На момент live gate: public, non-archived, license metadata `Other`, `2,704` stars, pushed `2026-05-28 12:23:22 UTC`, updated `2026-05-28 12:23:26 UTC`. Repo metadata: terminal agent multiplexer. Topics include `agent-orchestration`, `ai-agents`, `claude-code`, `codex`, `coding-agents`, `terminal-multiplexer`, `terminal-ui`, `tmux`, `tui`, `workspace-manager`. Local README/watch search found no prior `ogulcancelik/herdr` entry. | Repo-scope `WATCH`; high-fit terminal/control-plane surface, but no issue-level candidate selected. |
| `herdr#334/#333/#330/#318/#315/#303/#302` | Open issues are mostly labeled `intends-to-pr` or feature/control-plane work. `#332` was reported by fresh discovery as already closed/implemented pending release. Open PR list returned empty in parent gate. | `WATCH`; process gate likely expects intent/approval and a bounded issue selection first. |
| `microsoft/apm` | Новый repo-scope сигнал. Public, non-archived, MIT, `2,654` stars, pushed `2026-05-28 12:23:28 UTC`, updated `2026-05-28 12:27:47 UTC`. Repo metadata: Agent Package Manager. Topics include `ai-agents`, `codex-cli`, `context-engineering`, `prompt-engineering`, `claude-code`, `github-copilot`, `package-manager`. Local README/watch search found no prior `microsoft/apm` entry. | Repo-scope `WATCH`; strategic skills/package ecosystem surface, but no PR-ready lane. |
| `apm#1525/#1507/#1503/#1482` and PR queue | Open issues include update-command UX, local-scope integrate hangs in large repos, target leakage, and prompt install bugs. Open PR queue is active and broad: `#1504`, `#1478`, `#1464`, `#1424`, `#1422`, and others. | `WATCH`; future pass should pick one small CLI/install bug, then source/repro/process gate. |
| `vm0-ai/vm0` | Новый repo-scope сигнал. Public, non-archived, license metadata `Other`, `1,116` stars, pushed `2026-05-28 12:24:12 UTC`, updated `2026-05-28 12:24:16 UTC`. Repo metadata: AI teammate/runtime for real work. Topics include `agentic-workflow`, `ai-agent`, `ai-runtime`, `claude-code`, `sandbox`, `ai-sandbox`. Local README/watch search found no prior `vm0-ai/vm0` entry. | Repo-scope `WATCH`; relevant AI runtime/sandbox/computer-use surface, but no issue-level candidate. |
| `vm0#15280/#15279/#15278/#15277/#15276/#15275` and PR queue | Computer-use issue cluster is open and unassigned in part, but issue creation predates cutoff and repo PR queue is active. Nearby PRs include `#15311`, `#15303`, `#15302`, `#15291`, and `#15283`, with owner/maintainer assignments on several related issues. | `WATCH`; promising later CUA/runtime lane, but needs source checkout, duplicate search and runner/desktop evidence. |
| `anthropics/claude-code#63113` | Fresh product bug: plugin marketplace rejects `displayName`, labeled `bug`, `has repro`, `area:plugins`, no assignee. Public fix surface is unclear; possible related metadata came from `anthropics/claude-plugins-official`, but exact ownership path is not proven. | `LEAD/WATCH`; product/plugin schema lane, not candidate until public patch surface is verified. |
| `superset-sh/superset#4974/#4975/#3257` | Fresh `#4974` closed; `#4975` draft PR refs it, and older `#3257` overlaps OSC 8 terminal hyperlinks. | `NO-GO`; PR-covered/closed. |
| `opencode#29734/#29735`, `vercel/ai#15670/#15669`, `vercel/ai#15668`, `open-design#3202/#3208/#3210`, `agent-of-empires#1560/#1571`, `cmux#4938/#4939` | Fresh-looking lanes are assigned, merged, duplicate-covered, or active PR-covered. | `WATCH/NO-GO`; do not race. |

Carry-forward backlog stayed open but did not transition to `COMMENT-FIRST` or `PR-READY`: `probe#568`, `claude-peers-mcp#64/#65`, `google-gemini/gemini-cli#27503`, `vercel/ai#15652/#15668`, `simular-ai/Agent-S#195`, `browseros-ai/BrowserOS#1005`, `generalaction/emdash#1875`, `topoteretes/cognee#2907`, `agentmemory#703-#708`, `OpenViking#2256`, plus repo-scope `cmux`, `open-design`, `clawd-on-desk`, `agent-of-empires`, `MemOS`, and `hapi`.

## 6-role synthesis

- Fresh discovery selected `herdr` and `microsoft/apm` as material repo-scope records; `MoonshotAI/kimi-code` and `repowise-dev/repowise` are lower-priority `WATCH/LEAD`.
- Active backlog refresh found changed lanes only as `WATCH/NO-GO`: `vercel#15670 -> #15669`, `open-design#3166` closed/claimed, `open-design#3210` active PR, `agent-of-empires#1571` merged, `agent-of-empires#1560` approved/covered.
- Duplicate/race guard found no lane that can move above `CANDIDATE`; `superset#4974` is closed and PR-covered, `opencode#29734/#29735` is process-gated, and `claude-code#63113/#63114` are product-side.
- Process gates confirmed runner remains unavailable and no lane may move to `COMMENT-FIRST` or `PR-READY`.
- Actionability kept the no-runner order as `claude-peers-mcp#64`, `Agent-S#195`, `vercel/ai#15652`, `gemini-cli#27503`; true runner order remains `probe#568`, `claude-peers-mcp#64`, `gemini-cli#27503`, `vercel/ai#15652`, `Agent-S#195`.
- Scope/materiality selected `herdr`, `microsoft/apm`, and `vm0` as material repo-scope deltas after the 12:22 `MemOS/hapi` watch.

## Critique

- Factology/duplicates: record `herdr`, `microsoft/apm`, and `vm0` only as repo-scope `WATCH`. Star counts, topics, license metadata and push/update times are live snapshots. GitHub Search hit rate limits in subagent work, so issue-level duplicate proof is incomplete.
- Process gates: internal watch/tracker update is allowed. Upstream action is blocked by no bounded issue selection, no source checkout, no runner/current-main repro and no pre-action 3-role critique for a concrete patch.
- Actionability: `vm0` has interesting computer-use issues, but several nearby issues are assigned and PR queue is active. `herdr` and `apm` require a focused future issue selection before source work.

## Next

Keep `ogulcancelik/herdr`, `microsoft/apm`, and `vm0-ai/vm0` on repo-scope watch. Do not open upstream comments or PRs from this cycle.
