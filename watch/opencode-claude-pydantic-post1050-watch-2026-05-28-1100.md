# opencode / Claude / pydantic post-10:50 watch, 2026-05-28 11:00 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52), comment [#4563355498](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4563355498)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are still not installed in this environment, so this cycle used the documented fallback plus a 6-role read-only subagent cycle after clearing completed agent handles: fresh discovery, active candidate refresh, duplicate/PR race, process gates, actionability, and repo-scope guard.

No upstream comment, PR, ping, rerun, or runner action was made.

`next_status: CANDIDATE`

Upstream action count: `0`.

Runner action count: `0`.

## Parent live gates

Runner gate remains closed via [#10](https://github.com/serejaris/corp-opensource/issues/10): no dedicated `corp-opensource-runner` is provisioned. CT `216` remains Hermes-only and was not used.

| Lane | Live result | Decision |
|---|---|---|
| `anomalyco/opencode#29726` | Fresh source-level report: `BlockAnchorReplacer` in `packages/opencode/src/tool/edit.ts` uses `SINGLE_CANDIDATE_SIMILARITY_THRESHOLD = 0.0`, and the edit path reads at line 122 then writes at line 151 without an external mutation guard. Exact open PR search found no cover, but the issue is already assigned to `nexxeln`. Adjacent edit-tool lanes exist (`#23517`, `#24913`, `#26600`) but are not exact covers. | `CANDIDATE/WATCH-needs-source-test + assignment gate`; no PR/comment. |
| `anthropics/claude-code#63088` | Fresh detailed worktree cleanup bug with repro and labels `bug`, `has repro`, `platform:macos`, `area:core`, `area:agents`. It is adjacent to `#63087` and older worktree cleanup cluster `#26725/#35862/#27753`; public code search did not find `createAgentWorktree` / `cleanupStaleAgentWorktrees` source. | `WATCH / product-owned + duplicate-adjacent`; no public patch lane. |
| `anthropics/claude-code#63087` | Fresh stale worktree disk-space leak. Duplicate bot links `#26725/#55435` and auto-close window; `#63088` is the stronger fresh sibling. | `WATCH / duplicate-risk`. |
| `anomalyco/opencode#29725` | Docs locale selector bug, assigned to `Brendonovich`; duplicate bot links `#14409/#15904/#15984/#22415/#15194`. | `NO-GO / docs duplicate + assignment`. |
| `pydantic/pydantic-ai#5699` | Fresh open author PR for `#5629`, preserving merged request metadata; mostly green CI but still in progress at check time. Existing open author PR `#5692` already covers the same issue family. | `WATCH / author-owned PR cover`; do not race. |
| `vercel/ai#15668` | Rechecked from previous heartbeat: exact open PR `#14260` changes the same `activeResponse!.state.message` path to optional chaining; `#13880` covers adjacent concurrent `resumeStream` coalescing. | `WATCH / duplicate-covered`. |
| `google-gemini/gemini-cli#27518` | Rechecked EBADF `resizePty` lane; fresh Linux confirmation remains useful, but open PRs `#27371/#27429/#27502/#27372/#27496` cover the EBADF cluster. | `WATCH / duplicate-PR-covered`. |
| `langchain-ai/langchain#37732/#37733/#37734` | Author added a detailed approach and assignment request after 10:50; `#37734` remains closed/unmerged due `missing-issue-link` / assignment process. | `WATCH / author-owned closed-process`; do not race. |

Carry-forward runner candidates had no transition after 10:50: `probe#568`, `louislva/claude-peers-mcp#64`, `vercel/ai#15652`, `google-gemini/gemini-cli#27503`, and `simular-ai/Agent-S#195` remain `CANDIDATE-needs-runner-validation` or `needs-runner-fixture`. `opencode#26187` remains conditional `CANDIDATE/WATCH-needs-source-test` but assigned to `rekram1-node`.

## 6-role synthesis

- Fresh discovery found `opencode#29726`, `claude-code#63087/#63088`, `opencode#29725`, and `pydantic-ai#5699`; no fresh created-after-cutoff issue-level signal in Codex, LangChain, Gemini SearchText, Probe, Claude-peers, Agent-S, Playwright MCP, MCP SDKs, or ChromeDevTools MCP.
- Active candidate refresh found no state transition for the core runner backlog. `langchain#37732/#37734` became more clearly author-owned after the author requested assignment.
- Duplicate/race review initially overpromoted `vercel#15668`, but parent direct PR gate corrected it to exact-covered by `#14260`.
- Process gates allow only internal tracking: fresh `opencode#29726` is assigned, Claude worktree cleanup is product/duplicate-adjacent, and pydantic metadata PRs are author-owned.
- Actionability ranking for a future dedicated runner remains: `probe#568`, `claude-peers-mcp#64`, `gemini-cli#27503`, `vercel/ai#15652`, `Agent-S#195`; `opencode#29726` is a new source-level watch behind those because of assignment/process gates.

## 3-role critique

- Factology/duplicates: do not claim `vercel#15668` uncovered; `#14260` is exact cover. Do not treat `gemini#27518` as clean; EBADF PR cluster is active. Do not race `langchain#37734` or `pydantic#5699/#5692`.
- Process gates: no upstream action is justified. `opencode#29726` is interesting but assigned; any future move needs source-test evidence and maintainer/process clarity.
- Actionability: final backlog status remains `CANDIDATE` because existing source-level candidates are open and not exact-covered, but no fresh lane is `COMMENT-FIRST` or `PR-READY`.

## Next

Keep `probe#568` first when the dedicated runner appears. Watch `opencode#29726` for maintainer comments, assignment progress, or an exact PR. Re-enter only after runner/source-test evidence and a repeat duplicate/process gate.
