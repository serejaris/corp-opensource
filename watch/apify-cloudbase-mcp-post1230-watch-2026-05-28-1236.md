# Apify / CloudBase MCP post-12:30 watch, 2026-05-28 12:36 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Обязательные skills `open-source-bug-scouting` / `open-source-pr-workflow` в этом окружении всё ещё не установлены. Цикл выполнен через documented fallback: 6 read-only subagent ролей, parent live gates и duplicate/process synthesis.

Upstream comment, PR, ping, rerun или runner action не выполнялись.

`next_status: WATCH`

Fresh repo-scope delta:

- `apify/apify-mcp-server` = repo-scope `WATCH`.
- `TencentCloudBase/CloudBase-MCP` = repo-scope `WATCH`.

Upstream action count: `0`.

Runner action count: `0`.

## Parent live gates

Runner gate остаётся закрытым через [#10](https://github.com/serejaris/corp-opensource/issues/10): dedicated `corp-opensource-runner` не provisioned. CT `216` остаётся Hermes-only и не использовался для general repro.

| Lane | Live result | Decision |
|---|---|---|
| `apify/apify-mcp-server` | Новый repo-scope сигнал. На момент live gate: public, non-archived, MIT, `1,282` stars, pushed `2026-05-28 12:34:15 UTC`, updated `2026-05-28 12:34:19 UTC`. Repo metadata: MCP server that lets agents use Apify Store scrapers/crawlers/automation tools. Topics include `agents`, `ai`, `mcp`, `mcp-server`. Local README/watch search found no prior `apify/apify-mcp-server` entry. | Repo-scope `WATCH`; strong MCP/tool-runtime surface, but no issue-level candidate selected. |
| `apify#924/#918/#917/#913/#912/#911/#907` and PR queue | Open issue surface includes protocol compliance, structuredContent/outputSchema conflicts, TOON format, storage tools and test coverage. Some issues are assigned to the AI team; open PRs include `#928`, `#926`, `#914`, `#794`, `#680`. | `WATCH`; `#917` may become a future source-local lead, but needs focused source checkout, duplicate search and regression card. |
| `TencentCloudBase/CloudBase-MCP` | Новый repo-scope сигнал. Public, non-archived, MIT, `1,025` stars, pushed `2026-05-28 12:32:38 UTC`, updated `2026-05-28 12:32:19 UTC`. Repo metadata: CloudBase MCP connects CloudBase to AI agents and prompt-to-live-app workflows. Topics include `mcp`, `aicoding`, `vibe-coding`, `cursor`, `serverless`, `web`, `cloudbase`. Local README/watch search found no prior `TencentCloudBase/CloudBase-MCP` entry. | Repo-scope `WATCH`; relevant MCP/deployment/devtool surface, but no issue-level candidate. |
| `CloudBase-MCP#539/#533/#515/#514/#462` and PR queue | Open issues are older and heavily AI-automation labeled (`ai-processed`, `ai-fix`, `ai-failed`, `ai-processing`). Open PR queue is also older and owner/automation-heavy with many CloudBase tool/schema fixes. | `WATCH`; no clean post-cutoff lane, high process/automation collision risk. |
| `anthropics/claude-code#63116/#63117/#63115/#63113` | Fresh product issues after cutoff. `#63117/#63115` are duplicate-labeled; `#63116` is enhancement/product plugin context loading; `#63113` remains plugin schema `LEAD/WATCH`, but public fix surface is unproven. | `WATCH/NO-GO`; product/private-surface gate. |
| `opencode#29734/#29735`, `agent-of-empires#1549/#1579/#1580`, `cmux#4815/#4429`, `vercel#15670/#15669`, `vercel#15668` | Fresh-looking lanes are assigned, merged, duplicate-covered, PR-covered, or feature/process-heavy. | `WATCH/NO-GO`; do not race. |

Carry-forward backlog stayed open but did not transition to `COMMENT-FIRST` or `PR-READY`: `probe#568`, `claude-peers-mcp#64/#65`, `google-gemini/gemini-cli#27503`, `vercel/ai#15652/#15668`, `simular-ai/Agent-S#195`, `browseros-ai/BrowserOS#1005`, `generalaction/emdash#1875`, `topoteretes/cognee#2907`, `agentmemory#703-#708`, `OpenViking#2256`, plus repo-scope watches from prior cycles.

## 6-role synthesis

- Fresh discovery and scope/materiality independently selected `apify/apify-mcp-server` and `TencentCloudBase/CloudBase-MCP` as material repo-scope `WATCH` deltas.
- Active backlog refresh found only `WATCH/NO-GO` changes: `agent-of-empires#1549` merged, `#1579/#1580` assigned, `cmux#4815/#4429` active PR churn, `apm#1527` occupied feature PR, and `vm0#15312` release automation.
- Duplicate/race guard kept fresh signals out of PR lanes: `opencode#29734` is exact-covered by `#29735`, Claude Code issues are duplicate/product/private-surface, and Vercel/open-design/agent-of-empires lanes remain covered.
- Process gates confirmed runner unavailable and no lane may move to `COMMENT-FIRST` or `PR-READY`.
- Actionability kept no-runner order as `claude-peers-mcp#64`, `Agent-S#195`, `vercel/ai#15652`, `gemini-cli#27503`; true runner order remains `probe#568`, `claude-peers-mcp#64`, `gemini-cli#27503`, `vercel/ai#15652`, `Agent-S#195`.

## Critique

- Factology/duplicates: record both repos only as repo-scope `WATCH`. Star counts, topics, license and push/update times are live snapshots. Neither repo has source checkout, issue-level duplicate proof or fail-before repro in this cycle.
- Process gates: internal watch/tracker update is allowed. Upstream action is blocked by no bounded issue selection, no runner/source evidence and no pre-action 3-role critique for a concrete patch.
- Actionability: `apify#917` is the most plausible later source-local bug lead, but active AI-team ownership and output-schema/protocol nuance require a focused future pass.

## Next

Keep `apify/apify-mcp-server` and `TencentCloudBase/CloudBase-MCP` on repo-scope watch. Do not open upstream comments or PRs from this cycle.
