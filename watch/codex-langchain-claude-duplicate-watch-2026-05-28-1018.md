# Codex / LangChain / Claude duplicate watch, 2026-05-28 10:18 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52), comment [#4563053886](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4563053886)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are still not installed in this environment, so this cycle used the documented fallback: parent live GitHub gates, 6 read-only subagent roles, duplicate/PR searches, and 3-role critique before status selection.

No upstream comment, PR, ping, rerun, or runner action was made.

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

## Parent live gates

Runner gate remains closed via [#10](https://github.com/serejaris/corp-opensource/issues/10): no dedicated `corp-opensource-runner` is provisioned. CT `216` remains Hermes-only and was not used.

Fresh post-10:14 UTC signals were checked with live issue/PR views and targeted duplicate searches:

| Lane | Live result | Decision |
|---|---|---|
| `openai/codex#24890` | Fresh open MCP OAuth report: `codex mcp login` discovers OAuth protected resource metadata but omits the `resource` parameter on the authorize URL, producing a wrong token audience. Parent duplicate gate found open exact/strong duplicate `#13891` with labels `bug`/`auth`/`mcp`, extensive reproductions, and fork-fix branches/comments. PR search for the resource-indicator fix returned no official PR. | `WATCH / duplicate-covered-by-open-issue+fork-fixes`; not a new candidate and no upstream comment. |
| `langchain-ai/langchain#37724` / `#37731` | Open issue for OpenAI Responses image `file_id` support. PR `#37731` was opened and closed unmerged within the same minute; earlier proposed fix `#37725` is also closed. The lane is author-owned/assignment-gated. | `WATCH / assignment-gated closed PR cover`; no racing PR or comment. |
| `anthropics/claude-code#63078` | Fresh `Cannot modify thinking blocks` API 400 report, already labeled `duplicate`; search found a broad exact/near-exact cluster including `#63072`, `#10199`, `#41992`, `#20699`, `#62965`, and older closed reports. | `NO-GO / duplicate-cluster`; keep out of implementation queue. |

Carry-forward source-level candidates had no status transition after the previous 10:12 UTC note: `claude-peers-mcp#64` remains the best compact `CANDIDATE-needs-runner-validation`; `vercel/ai#15652`, `probe#568`, and `gemini-cli#27503` remain runner-backed candidates; `opencode#29717/#29718` remain active external PR covers blocked by upstream process/compliance gates.

## 6-role synthesis

- Fresh discovery surfaced `codex#24890`, `langchain#37724/#37731`, and `claude-code#63078`, but parent live gates corrected all three away from implementation.
- Active-refresh and actionability roles found no stronger post-10:14 transition in existing tracked lanes.
- Duplicate/race role confirmed that opencode permission/MCP inheritance fixes are already covered by active PRs, and that LangChain assignment/ownership lanes should not be raced.
- Process role kept the status at internal `WATCH`: runner is unavailable, required skills are unavailable, and no lane meets `COMMENT-FIRST` or `PR-READY`.

## 3-role critique

- Factology/duplicates: approved after correcting the fresh Codex lead; `#24890` must be treated as duplicate/strong duplicate of open `#13891`, not as uncovered.
- Process gates: internal tracker/watch update is allowed; upstream comment/PR is blocked because no lane is uncovered, runner-backed, and process-clear.
- Actionability: final status remains `WATCH`; no current-main repro was attempted without the dedicated runner.

## Next

Watch `codex#13891/#24890` only for an official maintainer-selected fix or a new PR that can be reviewed without racing fork authors. Watch LangChain image `file_id` only for maintainer assignment/reopen or a maintainer-authored replacement. Ignore Claude thinking-block reports unless upstream asks for fresh evidence.
