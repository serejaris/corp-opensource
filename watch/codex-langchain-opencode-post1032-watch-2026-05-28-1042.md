# Codex / LangChain / opencode post-10:32 watch, 2026-05-28 10:42 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52), comment [#4563256336](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4563256336)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are still not installed in this environment, so this cycle used the documented fallback: parent live GitHub gates, 6 read-only subagent roles, duplicate/PR searches, and process/actionability/duplicate critique.

No upstream comment, PR, ping, rerun, or runner action was made.

`next_status: CANDIDATE`

Upstream action count: `0`.

Runner action count: `0`.

## Parent live gates

Runner gate remains closed via [#10](https://github.com/serejaris/corp-opensource/issues/10): no dedicated `corp-opensource-runner` is provisioned. CT `216` remains Hermes-only and was not used.

| Lane | Live result | Decision |
|---|---|---|
| `openai/codex#24896` | Fresh open issue for rapid usage-limit drain and abnormal context-window usage on `codex-cli 0.134.0`, `gpt-5.5`, Linux. Search found related open usage/context cluster `#24337/#23136/#14593`, and no exact PR cover. Evidence is a doctor report plus logs from one environment; this may be backend/account/model accounting rather than a compact CLI patch lane. | `LEAD / WATCH`; no upstream comment/PR without independent repro or maintainer ask. |
| `anthropics/claude-code#63082` | Post-10:32 duplicate bot linked `#56172`; reporter replied that `#63082` is distinct because the startup scanner deletes an existing valid `cliSessionId`, while `#56172` is about missing metadata. Still product-owned Desktop/session data-loss surface. | `WATCH / product-owned + duplicate-risk`; no public OSS patch lane. |
| `anomalyco/opencode#29723` | Fresh feature request for cloning repositories from URL in new project flow. Assigned to `jlongster`, `needs:compliance`, duplicate bot points to open assigned `#18241`; old clone-dialog PR `#16227` is closed unmerged. | `NO-GO / feature + duplicate + compliance + assignment`. |
| `anomalyco/opencode#29724` | Billing/refund/support request, assigned to `fwang`; duplicate/support search finds other refund issues. | `NO-GO / support-refund`. |
| `langchain-ai/langchain#37732` | Technically plausible `RecursiveJsonSplitter` `max_chunk_size` bug with MRE, but issue was auto-closed within a minute as programmatic/non-template submission. PR search for the exact path-overhead bug found no cover. | `NO-GO for upstream action`; possible future `LEAD` only if manually refiled or independently reproduced in runner. |

Carry-forward runner candidates had no transition after 10:32: `probe#568`, `claude-peers-mcp#64`, `vercel/ai#15652`, and `gemini-cli#27503` remain `CANDIDATE-needs-runner-validation`. `Agent-S#195` is still the cheapest practical fallback after Probe/Claude-peers if the runner has Python/unit-test tooling before Node/Pnpm or browser tooling.

## 6-role synthesis

- Fresh discovery found `codex#24896` as a weak usage/accounting lead, `langchain#37732` as auto-closed technical no-go, and opencode `#29723/#29724` as feature/support no-go.
- Active-lane refresh found no post-10:32 transition for core runner candidates, and kept `opencode#26187` as conditional assigned runner/source-test candidate.
- Duplicate/race critique kept Claude `#63082` in watch after duplicate-bot link to `#56172`, and kept opencode `#29723/#29724` out of the queue.
- Process critique allowed internal tracker/watch update only; upstream comment/PR and general runner actions remain blocked.
- Actionability critique kept queue order: `probe#568`, `claude-peers-mcp#64`, `Agent-S#195`, then `vercel/ai#15652` / `gemini-cli#27503` depending on runner tooling.

## 3-role critique

- Factology/duplicates: do not call `claude-code#63082` clean/uncovered after duplicate-bot linked `#56172`; do not call `codex#24896` a candidate without independent repro.
- Process gates: no upstream comment, PR, rerun, or runner action is allowed.
- Actionability: final backlog status remains `CANDIDATE`, but no fresh lane is `COMMENT-FIRST` or `PR-READY`.

## Next

Keep `probe#568` first for the future dedicated runner. Recheck `codex#24896` only if it gains maintainer confirmation, multiple independent reproductions, or a source-local accounting path. Ignore opencode refund/support and duplicate feature lanes. Re-enter `langchain#37732` only after a manual issue or runner-backed current-main repro.
