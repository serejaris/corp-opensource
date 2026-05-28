# Vercel / LangChain / Gemini post-10:42 watch, 2026-05-28 10:50 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52), comment [#4563303195](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4563303195)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are still not installed in this environment, so this cycle used the documented fallback: parent live GitHub gates, 6 read-only subagent roles, duplicate/PR searches, and process/actionability/duplicate critique.

No upstream comment, PR, ping, rerun, or runner action was made.

`next_status: CANDIDATE`

Upstream action count: `0`.

Runner action count: `0`.

## Parent live gates

Runner gate remains closed via [#10](https://github.com/serejaris/corp-opensource/issues/10): no dedicated `corp-opensource-runner` is provisioned. CT `216` remains Hermes-only and was not used.

| Lane | Live result | Decision |
|---|---|---|
| `vercel/ai#15668` | Fresh open issue for `Chat.makeRequest` throwing from `this.activeResponse!.state` during `resumeStream` overlap. Parent duplicate gate found exact/strong open PR cover: `#14260` changes the same `activeResponse!.state.message` line to optional chaining, and `#13880` covers concurrent `resumeStream` coalescing. | `WATCH / duplicate-covered`; no PR/comment. |
| `langchain-ai/langchain#37733/#37734` | `#37733` repeats the auto-closed `RecursiveJsonSplitter` path-overhead bug from `#37732`; `#37734` is an author-owned fix PR touching `json.py` and tests, but it was auto-closed because the author was not assigned to the linked issue and carries `missing-issue-link`. | `WATCH / author-owned closed process gate`; do not race. |
| `google-gemini/gemini-cli#27518` | Fresh confirmation comment at 10:47 UTC ties the EBADF `resizePty` crash to quota-exhaustion shutdown on Linux. Existing bot duplicate cluster remains broad, and parent PR gate found multiple open EBADF PRs: `#27371/#27429/#27502/#27372/#27496`. | `WATCH / duplicate-PR-covered`; not our lane. |
| `anthropics/claude-code#63084` | Product/model behavior report with duplicate-bot links to `#54360/#24585` and auto-close warning. | `WATCH / product-owned duplicate-risk`; no public patch lane. |
| `anthropics/claude-code#63085` | Enhancement request for naming/renaming background agents. | `NO-GO / product feature`. |
| `anomalyco/opencode#29723/#29724` | Same post-10:32 feature/support lanes as previous heartbeat. `#29723` is assigned and duplicate-linked to `#18241`; `#29724` is billing/refund/support. | `NO-GO`; no new candidate delta. |

Carry-forward runner candidates had no transition after 10:42: `probe#568`, `louislva/claude-peers-mcp#64`, `vercel/ai#15652`, `google-gemini/gemini-cli#27503`, and `simular-ai/Agent-S#195` remain `CANDIDATE-needs-runner-validation` or `needs-runner-fixture`. `opencode#26187` remains conditional `CANDIDATE/WATCH-needs-source-test` but assignment-gated.

## 6-role synthesis

- Fresh discovery found `langchain#37733/#37734` as the strongest new technical signal, but already author-owned and process-closed.
- Duplicate/race review found `vercel/ai#15668` exact-covered by older open PR `#14260`, with adjacent concurrency cover in `#13880`.
- Active candidate refresh found no state transition for `probe#568`, `claude-peers-mcp#64`, `vercel/ai#15652`, `gemini-cli#27503`, `Agent-S#195`, or `opencode#26187`.
- Process gates allow only internal tracking. No upstream comment, PR, rerun, or runner action is allowed.
- Actionability ranking for a future dedicated runner remains: `probe#568`, `claude-peers-mcp#64`, `gemini-cli#27503`, `vercel/ai#15652`, `Agent-S#195`, then `opencode#26187`.

## 3-role critique

- Factology/duplicates: do not promote `vercel/ai#15668`; `#14260` is an exact open cover. Do not promote `gemini-cli#27518`; multiple open EBADF PRs exist. Do not race `langchain#37734`.
- Process gates: no upstream action is justified; runner remains unavailable.
- Actionability: final backlog status remains `CANDIDATE` because existing source-level candidates are open and not exact-covered, but no fresh lane is `COMMENT-FIRST` or `PR-READY`.

## Next

Keep `probe#568` first when the dedicated runner appears. Recheck `langchain#37734` only if maintainers assign/reopen or a clean template issue appears. Watch `vercel/ai#14260/#13880` for whether they settle `#15668`; do not open a duplicate PR.
