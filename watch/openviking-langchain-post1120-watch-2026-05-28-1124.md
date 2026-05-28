# OpenViking / LangChain post-11:20 watch, 2026-05-28 11:24 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are still not installed in this environment, so this cycle used the documented fallback plus a 6-role read-only subagent cycle: fresh discovery, active candidate refresh, duplicate/PR race, process gates, actionability, and scope/materiality guard.

No upstream comment, PR, ping, rerun, or runner action was made.

`next_status: CANDIDATE`

Upstream action count: `0`.

Runner action count: `0`.

## Parent live gates

Runner gate remains closed via [#10](https://github.com/serejaris/corp-opensource/issues/10): no dedicated `corp-opensource-runner` is provisioned. CT `216` remains Hermes-only and was not used.

GitHub Search API returned 403 rate-limit errors during broad search. Direct issue, PR, repo, and focused exact PR views below were available.

| Lane | Live result | Decision |
|---|---|---|
| `volcengine/OpenViking` | New repo-scope signal. Public, active Python repo; `24,818` stars, `1,889` forks, AGPL-3.0, default `main`, pushed `2026-05-28 11:19:19 UTC`. Description: context database for AI agents, with topics including `context-engineering`, `memory`, `skill`, `agent`, `ai-agents`, `openclaw`, `opencode`. Root has `CONTRIBUTING.md`, `CONTRIBUTING_CN.md`, `CONTRIBUTING_JA.md`, and `SECURITY.md`. | Add repo-scope `WATCH`; high fit for Paperclip-like memory/context strategy, but no upstream action. |
| `volcengine/OpenViking#2256` | Open, unassigned, no labels/comments. Parent/source critique confirms `FindRequest` and `SearchRequest` in `openviking/server/routers/search.py` lack `mode`, `/find` does not pass `mode` into `service.search.find`, and `RetrieverMode` exists in the retriever layer. Focused open PR search for `FindRequest mode search find` returned empty. Caveat: the Hermes Agent / external client schema claim was not verified inside the OpenViking repo, so any upstream action needs a source/repro pass that verifies the client boundary too. | `LEAD/WATCH`; promising source-local API contract lead, but needs source checkout, repro/test card, client-boundary verification, and repeat PR/duplicate gate before any comment/PR. |
| `volcengine/OpenViking#2261/#2262` | `#2261` is open, but exact author-owned PR `#2262` is open and review-required. | `NO-GO / covered`. |
| `volcengine/OpenViking#2263/#2268` | `#2263` is open, but exact/strong PR `#2268` binds storage and crypto keys to security context; open and review-required. | `NO-GO / covered`. |
| `langchain-ai/langchain#37735` | Fresh issue created `2026-05-28 11:20:14 UTC`, closed by bot at `11:20:26 UTC` as programmatic submission/no issue type. Labels `langchain`, `external`; unassigned. The report is self-contained: `EnsembleRetriever.arank_fusion` wraps any non-`Document` as `Document(page_content=doc)`, unlike sync `rank_fusion` which wraps only strings, causing async `ValidationError` for non-string values. Focused open PR search for `arank_fusion EnsembleRetriever ValidationError Document page_content` returned empty. | `LEAD / process-blocked`; technically source-local and regression-testable, but closed issue process gate blocks upstream action. |
| `openai/codex#24900` | Fresh app issue: Codex Desktop cannot spawn subagents because it believes no `spawn_agent` tool exists. Product/app surface, no public patch path established. | `WATCH`, not candidate. |
| `anthropics/claude-code#63097/#63098` | `#63097` is duplicate-labeled Desktop permissions mode flip; `#63098` is a hook workaround/enhancement for model weekday mistakes. | `NO-GO/WATCH`; product/duplicate/enhancement lanes. |

Carry-forward runner candidates had no transition after 11:20: `probe#568`, `louislva/claude-peers-mcp#64`, `vercel/ai#15652`, `google-gemini/gemini-cli#27503`, `simular-ai/Agent-S#195`, `browseros-ai/BrowserOS#1005`, and `generalaction/emdash#1875` remain `CANDIDATE-needs-runner-validation` or `needs-runner-fixture`. `opencode#29726` and `opencode#26187` remain assignment-gated watch/candidates.

## 6-role synthesis

- Fresh discovery found `langchain#37735` as the only new issue-level source-local signal after `11:20 UTC`; it is closed by LangChain process automation and cannot advance without a clean process path.
- Active candidate refresh found no state transition for the core runner backlog. `probe#568` remains the first practical runner target; `claude-peers-mcp#64` remains the compact source-level fallback.
- Duplicate/race review found fresh `opencode#29728`, `mcp/python-sdk#2706`, `mcp/python-sdk#2704/#2705`, and Codex memory PR lanes occupied by author-owned PRs or process gates.
- Process gates confirmed no upstream action: runner unavailable, broad search rate-limited, fresh Claude/Codex lanes are product/duplicate/app surfaces, and OpenViking needs a separate repo checkout/repro cycle.
- Actionability ranking for future runner remains: `probe#568`, `claude-peers-mcp#64`, `gemini-cli#27503`, `vercel/ai#15652`, `Agent-S#195`, then `BrowserOS#1005` / `emdash#1875` as environment-specific follow-ups.
- Scope guard recommends recording `volcengine/OpenViking` as new repo-scope watch because it is popular, active, AI-agent context/memory infrastructure and was not previously present in local watch/README.

## 3-role critique

- Factology/duplicates: `OpenViking` repo-scope `WATCH` is valid; `OpenViking#2256` stays `LEAD/WATCH` because source risk is plausible but the external client/schema boundary is not fully verified. `langchain#37735` is technically plausible and no exact open PR was found, but it is closed by process automation.
- Process gates: internal watch/tracker update is allowed. No upstream action is allowed because the runner is unavailable, broad search was rate-limited, `OpenViking#2256` lacks source/repro evidence, and `langchain#37735` is process-blocked.
- Actionability: keep overall heartbeat `next_status: CANDIDATE` only because carry-forward candidates remain open. For the new delta specifically: `OpenViking` repo `WATCH`, `OpenViking#2256` `LEAD/WATCH`, `langchain#37735` `LEAD/process-blocked`. Do not reorder the runner queue.

## Next

Keep `probe#568` first when the dedicated runner appears. For OpenViking, run a separate source-level cycle around `#2256`: checkout current `main`, inspect `server/routers/search.py`, plugin schema, and retriever mode plumbing, then repeat duplicate/PR search and contribution gates. For `langchain#37735`, do not open a cold PR from the closed issue; only revisit if a proper template issue appears, a maintainer asks, or a source-test cycle finds a process-safe path.
