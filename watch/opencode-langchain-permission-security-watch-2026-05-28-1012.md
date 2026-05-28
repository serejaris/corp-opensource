# opencode/LangChain permission-security watch, 2026-05-28 10:12 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562994890)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this used the documented fallback: parent live GitHub gates, reused 6 read-only subagent roles, and focused duplicate/PR checks before status selection.

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Runner gate: dedicated `corp-opensource-runner` is still unavailable via [#10](https://github.com/serejaris/corp-opensource/issues/10). CT216 remains Hermes-only and was not used.

## Material deltas

- `sst/opencode#29717`: fresh external PR for `#29674` (`**/.env*` deny rule and read/glob/grep enforcement). Open, non-draft, `BLOCKED`; compliance bot says the PR template is incomplete. Status: `WATCH / covered-by-PR`.
- `sst/opencode#29718`: fresh external PR for `#16491` (subagents inherit parent session allow rules for MCP tool permissions). Open, non-draft, `BLOCKED`; compliance bot says the PR template is incomplete. Status: `WATCH / covered-by-PR`.
- `langchain-ai/langchain#37296`: old public Chroma path traversal issue received fresh author assignment/attribution pings at `2026-05-28T10:10:25Z` and `10:11:52Z`. Cover PR `#37291` is closed/unmerged and `#37729` for adjacent `load_prompt()` path traversal is also closed/unmerged. Status: `WATCH / author-owned process gate`, not our lane.
- `anthropics/claude-code#63076`: duplicate-labeled session handoff/token budget feature. Status: `NO-GO / duplicate-feature`.

Carry-forward unchanged:

- `louislva/claude-peers-mcp#64` remains the best compact source-level `CANDIDATE-needs-runner-validation`.
- `vercel/ai#15652`, `probelabs/probe#568`, and `google-gemini/gemini-cli#27503` remain runner-backed candidates.
- `langchain-ai/langchain#37728` remains `WATCH / duplicate-covered` by `langchain-ai/langchain-google#1704/#1708`.

## Parent live gates

`opencode#29717`:

- State: open PR, non-draft, `mergeStateStatus: BLOCKED`.
- Body says it fixes `#29674`; changed files include wildcard utility, glob/grep/read permission plumbing, and permission tests.
- Issue `#29674` is open, assigned `jlongster`, and duplicate-bot links `#28150/#25268/#25515/#16331`.
- Decision: `WATCH / covered-by-PR`; no duplicate PR/comment while active PR and assignment gates are live.

`opencode#29718`:

- State: open PR, non-draft, `mergeStateStatus: BLOCKED`.
- Body says it fixes `#16491`; changed file is `packages/opencode/src/agent/subagent-permissions.ts`.
- Issue `#16491` is open, assigned `rekram1-node`, and duplicate-bot linked old `#3808`.
- Decision: `WATCH / covered-by-PR`; no duplicate PR/comment while active PR and assignment gates are live.

`langchain#37296`:

- State: open issue, labels `bug`, `chroma`, `external`, no assignee.
- Fresh comments ask maintainers to assign the original reporter and preserve attribution; the reporter explicitly asks for direct merge and no cherry-picks.
- PR `#37291` (`Chroma.add_images()` path traversal) is closed/unmerged, updated at `2026-05-28T10:10:41Z`.
- PR `#37729` (`load_prompt()` top-level path traversal) is closed/unmerged at `2026-05-28T10:08:57Z`.
- Decision: `WATCH / author-owned process gate`; do not race a public vulnerability lane with an author-owned PR history and attribution dispute.

## 6-role synthesis

- Fresh discovery found no new clean post-10:10 repo candidate. The only post-cutoff signal worth recording is `langchain#37296` process movement.
- Active refresh found no material state transition for the core candidates: `claude-peers-mcp#64`, `vercel/ai#15652`, `probe#568`, `gemini-cli#27503`, `CLIProxyAPI#3594`, or LangChain Google duplicate cover.
- Duplicate/race found `opencode#29717/#29718` as active cover, not fresh local implementation targets.
- Process/actionability kept final `next_status: WATCH`; runner absence and missing regression cards block `COMMENT-FIRST`/`PR-READY`.

## 3-role critique

- Factology/duplicates: internal watch update is allowed; do not overclaim closed LangChain PRs as available work because author ownership/process risk is explicit.
- Process gates: upstream comment/PR count stays `0`; no runner action.
- Actionability: next promotion still requires runner-backed validation for a source-level lane such as `claude-peers-mcp#64`, `vercel/ai#15652`, `probe#568`, or `gemini-cli#27503`.
