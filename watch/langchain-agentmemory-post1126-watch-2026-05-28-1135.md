# LangChain / agentmemory post-11:26 watch, 2026-05-28 11:35 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are still not installed in this environment, so this cycle used the documented fallback plus a 6-role read-only subagent cycle: fresh discovery, active candidate refresh, duplicate/PR race, process gates, actionability, and scope/materiality guard.

No upstream comment, PR, ping, rerun, or runner action was made.

`next_status: CANDIDATE`

Upstream action count: `0`.

Runner action count: `0`.

## Parent live gates

Runner gate remains closed via [#10](https://github.com/serejaris/corp-opensource/issues/10): no dedicated `corp-opensource-runner` is provisioned. CT `216` remains Hermes-only and was not used.

GitHub Search API was partially rate-limited during the cycle. Direct issue, PR, and repo views were available; focused exact PR searches worked for some material checks, but duplicate coverage is still weaker than a clean full-search pass and blocks promotion.

| Lane | Live result | Decision |
|---|---|---|
| `langchain-ai/langchain#37736/#37737` | Material transition from prior `#37735`: the author reopened the async `EnsembleRetriever.arank_fusion` normalization bug with the proper issue template as `#37736` at `2026-05-28 11:27:28 UTC`; labels `bug`, `langchain`, `langchain-classic`, `external`, unassigned. The same author opened PR `#37737` at `11:28:57`, touching `libs/langchain/langchain_classic/retrievers/ensemble.py` and `libs/langchain/tests/unit_tests/retrievers/test_ensemble.py`; it closed automatically at `11:29:29` because the author was not assigned to the linked issue. Focused open PR search for `arank_fusion EnsembleRetriever rank_fusion` returned empty after the close. | `WATCH / author-owned process gate`; do not race. Revisit only if maintainers assign the author/reopen `#37737`, close the lane without action, or ask for an alternate patch. |
| `rohitg00/agentmemory` | New repo-scope signal. Public, active TypeScript repo; `19,106` stars, `1,562` forks, default `main`, pushed `2026-05-28 10:38:26 UTC`. Description: persistent memory for AI coding agents. Topics include `agentmemory`, `agents`, `claude`, `claudecode`, `codex`, `copilot`, `cursor`, `harness`, `hermes`, `memory`, `openclaw`. Root has `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, and `SECURITY.md`. Local repo search found no prior `agentmemory` entry. | Add repo-scope `LEAD/WATCH`; high fit for Paperclip-like memory strategy, but no upstream action. |
| `rohitg00/agentmemory#703-#708` | Fresh pre-cutoff native-surface audit cluster for skills/prompts/rules/MCP across Warp, Continue, Cline, Zed, Droid, and tracking issue `#708`; open and unassigned. Focused open PR search for the native-surface audit terms returned empty, but the issues were created before the post-11:26 cutoff and need a separate source/repro cycle. | `LEAD/WATCH`; do not promote yet. Needs repo-scope scan, exact duplicate/PR gate after rate-limit clears, and source-level validation. |
| `anomalyco/opencode#24316/#27984` | Fresh comment at `2026-05-28 11:27:00 UTC` says the Qwen XML tool-call leakage fix worked. The issue is open and assigned to `rekram1-node`; linked author-owned PR `#27984` is open, non-draft, `DIRTY`, and touches `packages/llm/src/protocols/openai-chat.ts` plus tests. | `WATCH / occupied`; no duplicate PR/comment. |
| `anthropics/claude-code#63100/#63099` | Fresh product reports: WSL Bash wait-forever and Windows `C:/memfs` sandbox/security behavior. Public patch lane not established; both are Claude Code product/closed-source surfaces. | `WATCH`, not candidate. |
| `anomalyco/opencode#29729` | Feature request for system-wide config, assigned to `kitlangton`. | `NO-GO / feature + assigned`. |

Carry-forward runner candidates had no transition after 11:26: `probe#568`, `louislva/claude-peers-mcp#64`, `vercel/ai#15652`, `google-gemini/gemini-cli#27503`, `simular-ai/Agent-S#195`, `browseros-ai/BrowserOS#1005`, `generalaction/emdash#1875`, and `volcengine/OpenViking#2256` remain `CANDIDATE`/`LEAD` only after runner or source-validation evidence. `opencode#29726` and `opencode#26187` remain assignment-gated watch/candidates.

## 6-role synthesis

- Fresh discovery found the material LangChain transition: `#37736` is now a process-valid open issue, but the author already opened `#37737`, which closed due assignment gate. This is no longer merely a closed bot issue, but it is still author-owned/process-sensitive.
- Scope guard found `rohitg00/agentmemory` as a new high-fit AI-agent memory repo not present in local README/watch. It is repo-scope material, not an issue-level PR lane.
- Duplicate/race review downgraded the LangChain lane to `WATCH/NO-GO for us now` because the original author already supplied the fix and tests.
- Process gates keep `probe#568`, `claude-peers-mcp#64`, `gemini-cli#27503`, and `vercel/ai#15652` as the cleanest carry-forward candidates, but all require runner/source evidence and repeat duplicate gates.
- Actionability ranking for a future dedicated runner remains: `probe#568`, `claude-peers-mcp#64`, `gemini-cli#27503`, `vercel/ai#15652`, `Agent-S#195`, then environment-specific BrowserOS/emdash/OpenViking follow-ups.

## 3-role critique

- Factology/duplicates: `langchain#37736/#37737` is correctly `WATCH / author-owned process gate`; the author owns the fix/test attempt and open PR search found no other active cover. `agentmemory` repo-scope watch is valid, but `#703-#708` must remain repo-scope/source-scan leads, not a concrete fix path.
- Process gates: internal watch/tracker update is allowed. No upstream action is allowed because runner evidence is missing, LangChain is author/process-gated, and duplicate coverage is weakened by search rate-limit.
- Actionability: record the material transition and new repo-scope lead, but keep overall `next_status: CANDIDATE` only because carry-forward runner-gated candidates remain open. Do not change top runner order.

## Next

Do not open a competing LangChain PR while `#37736/#37737` is author-owned and process-gated. Add `agentmemory` to repo-scope watch; a future focused cycle should inspect native-surface audit issues `#703-#708`, MCP/core version drift `#668`, and Claude Code capture `#657` only after duplicate search recovers and a source checkout identifies a bounded regression target.
