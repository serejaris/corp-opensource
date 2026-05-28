# opencode / Gemini / Serena runner watch, 2026-05-28 10:26 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52), comment [#4563119534](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4563119534); Serena tracker [#84](https://github.com/serejaris/corp-opensource/issues/84#issuecomment-4563119552)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are still not installed in this environment, so this cycle used the documented fallback: parent live GitHub gates, 6 read-only subagent roles, duplicate/PR checks where rate limits allowed, and process/actionability/duplicate critique.

No upstream comment, PR, ping, rerun, or runner action was made.

`next_status: CANDIDATE`

Upstream action count: `0`.

Runner action count: `0`.

## Parent live gates

Runner gate remains closed via [#10](https://github.com/serejaris/corp-opensource/issues/10): no dedicated `corp-opensource-runner` is provisioned. CT `216` remains Hermes-only and was not used.

GitHub Search API started returning rate-limit errors around `2026-05-28 10:25 UTC`, so late duplicate/PR checks used direct `gh issue view`, `gh pr view`, `gh issue list`, and `gh pr list` evidence where possible. Do not treat this as a clean PR-ready duplicate gate.

| Lane | Live result | Decision |
|---|---|---|
| `anomalyco/opencode#26187` | Open, assigned to `rekram1-node`, updated `2026-05-28 10:20:53 UTC` by a second reporter confirming Desktop `v1.14.48` still crashes when `webfetch` uses `format: "text"` on HTML in Electron: `HTMLRewriter is not defined`. Parent live issue body points to `packages/opencode/src/tool/webfetch.ts` using Bun/Workers-only `HTMLRewriter`; workaround is `format: "markdown"`. Latest open PR list did not show an exact webfetch/HTMLRewriter cover; adjacent `#29530` is `format=null`, not Electron text extraction. | Add as `CANDIDATE-needs-runner-or-source-test`. It is not `PR-READY`: issue is assigned, duplicate search was rate-limit constrained, and Electron/Desktop or source-level test evidence is missing. |
| `oraios/serena#1519` | Closed at `2026-05-28 09:30:17 UTC`; maintainer said the dashboard host-port issue was fixed in `6dd3301e48`. | Remove from active runner backlog; status `SETTLED / upstream fixed`. No repro or upstream comment needed. |
| `google-gemini/gemini-cli#27516` and EBADF cluster | Fresh comment at `2026-05-28 10:21:28 UTC` asked for reproducibility details. The issue is already `status/possible-duplicate` and part of a large `resizePty` / `ioctl(2) failed, EBADF` cluster. Open PRs `#27429` and `#27371` both target EBADF handling in `ShellExecutionService.resizePty`; `#27429` adds focused tests and is `BLOCKED/REVIEW_REQUIRED`, while `#27371` is `DIRTY/REVIEW_REQUIRED`. | `WATCH / duplicate-covered`; do not race. This does not affect the separate `gemini-cli#27503` SearchText candidate, which remains `CANDIDATE-needs-runner-validation`. |
| `vercel/ai#15664/#15665` | Already-covered lane rechecked: issue `#15664` has exact author PR `#15665` to omit unsupported Vertex `functionCall.id` / `functionResponse.id` fields while preserving Gemini Developer API IDs. | `WATCH / author-owned exact PR cover`; no action. This was already recorded in the 08:16 covered refresh and remains unchanged. |
| `openai/codex#19336`, `anthropics/claude-code#61724/#63079/#63080` | Fresh product/app updates: Codex macOS menubar position comment, Claude VS Code resume crash/product UX reports. They are product or closed-source/app-surface leads, not compact public OSS patch lanes. | `WATCH / product-only`; no candidate promotion. |

Carry-forward runner order after this cycle:

1. `probelabs/probe#568` remains the first practical dedicated-runner job.
2. `louislva/claude-peers-mcp#64` remains the compact source-level fallback.
3. `vercel/ai#15652` and `google-gemini/gemini-cli#27503` remain higher-impact package/CLI candidates.
4. `anomalyco/opencode#26187` is added as a runner/source-test candidate behind the above because the issue is assigned and Desktop/Electron evidence is missing.
5. `oraios/serena#1519` is removed from the active runner queue.

## 6-role synthesis

- Fresh discovery found `opencode#26187` as the only new narrow implementation-shaped candidate; it needs runner/source evidence and a fresh duplicate gate after rate limits clear.
- Active-lane refresh found no close/merge transitions for `claude-peers-mcp#64`, `vercel/ai#15652`, `probe#568`, or `gemini-cli#27503`; `langchain#37731` only gained an author assignment/reopen request in a closed PR.
- Duplicate/race critique kept Codex MCP OAuth, LangChain image `file_id`, Claude thinking blocks, opencode permission PRs, and Gemini EBADF in `WATCH`/`NO-GO`.
- Process critique allowed internal tracker/watch updates only; upstream comment/PR and general runner actions remain blocked.
- Actionability critique removed `serena#1519` from runner backlog and kept `probe#568` first.

## 3-role critique

- Factology/duplicates: `opencode#26187` is not covered by visible open PRs in the latest list, but duplicate search is incomplete due rate limit and the issue is assigned; do not overclaim as clean/unassigned.
- Process gates: internal update is allowed; no upstream comment, PR, rerun, or runner action is allowed.
- Actionability: final backlog status is `CANDIDATE`, but no lane is `COMMENT-FIRST` or `PR-READY`.

## Next

When GitHub search recovers, rerun exact duplicate/PR search for `opencode#26187` (`HTMLRewriter`, `webfetch format text`, `Electron`) and inspect opencode contribution/compliance gates. If the dedicated runner becomes available, start with `probe#568`; use `opencode#26187` only after the higher-priority runner queue or if Desktop/Electron source-test evidence becomes cheap.
