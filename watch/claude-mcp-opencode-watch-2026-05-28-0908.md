# Claude/MCP/opencode watch, 2026-05-28 09:08 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this used the documented fallback: parent live GitHub gates, reused 6 read-only subagent roles, and focused duplicate/PR checks before status selection.

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Runner gate: dedicated `corp-opensource-runner` is still unavailable via [#10](https://github.com/serejaris/corp-opensource/issues/10). CT216 remains Hermes-only and was not used.

## Material deltas

- `anthropics/claude-code#63051`: API `529 Overloaded` issue gained `bug`, `duplicate`, `platform:macos`, `external`, `api:anthropic` labels and duplicate bot links to `#62188/#39763/#41628`; keep `NO-GO / duplicate-product-transient`.
- `anthropics/claude-code#63052`: fresh long-input TUI truncation report with screenshots on macOS/Ghostty; already labeled `duplicate`, `platform:macos`, `area:tui`, `area:ui`. Keep product `WATCH`, no external patch lane.
- `modelcontextprotocol/python-sdk#2695`: fresh follow-up proposal after PR `#2700` to add tests for the other reachable `convert_result=True` return shapes. This is a coordination/watch lane, not a cold PR.
- `anomalyco/opencode#26625`: fresh comment says `/timestamps` still has no visible effect on stable while PR `#29398` branch changes behavior; issue is assigned and maintainer says original bugs are not reproducible. Keep `WATCH / PR-linked ambiguity`.
- `vercel/ai#15667`: fresh docs-only OpenRouter link PR, non-draft `MERGEABLE/REVIEW_REQUIRED`; already covered and low Paperclip value.
- `JetBrains/koog#2079`: fresh question about `maxTokens: Int?` versus model context/output token fields; exact search only found older token-related PRs. Keep low-priority `LEAD/WATCH` until maintainers clarify a bug.

## Parent live gates

`claude-code#63051`:

- State: open, no assignee.
- Labels: `bug`, `duplicate`, `platform:macos`, `external`, `api:anthropic`.
- Duplicate bot links: `#62188/#39763/#41628`; auto-close in 3 days unless contested.
- Decision: `NO-GO / duplicate`.

`claude-code#63052`:

- State: open, no comments/assignee.
- Created: `2026-05-28T09:06:45Z`; updated: `2026-05-28T09:08:00Z`.
- Labels: `duplicate`, `platform:macos`, `area:tui`, `area:ui`.
- Body: long `<tab>` input lines collapse to ellipsis while typing instead of wrapping; screenshots included; Claude Code `2.1.145`, Ghostty.
- Focused search found only `#63052` by exact phrase.
- Decision: product `WATCH`.

`mcp/python-sdk#2695`:

- State: open issue.
- Fresh comment proposes follow-up tests for no-output-schema and direct `CallToolResult` reachable shapes after `#2700`.
- Existing thread says `#2700` already added the `ToolResult` alias and structured-output regression test.
- Decision: `WATCH / coordination-first`.

`opencode#26625`:

- State: open, assigned `kommander`.
- Fresh comment at `2026-05-28T09:07:28Z` says stable still shows no timestamps while the PR branch does.
- Existing PR link: `#29398`; maintainer had said the original bugs are no longer reproducible.
- Decision: `WATCH / PR-linked ambiguity`.

`vercel/ai#15667`:

- State: open PR, non-draft, `MERGEABLE`, `REVIEW_REQUIRED`.
- Scope: docs-only OpenRouter link path.
- Decision: `WATCH / already-covered`.

`JetBrains/koog#2079`:

- State: open, no labels/assignees/comments at gate time.
- Body asks whether `LLMParams.maxTokens: Int?` and model `contextLength/maxOutputTokens: Long?` semantics/types mismatch.
- Focused search found older token-related PRs `#734/#766/#776`, but no exact current cover.
- Decision: low-priority `LEAD/WATCH`, not candidate until clarified as a bug.

## 6-role synthesis

- Fresh discovery found no new source-level `CANDIDATE`; fresh material is product duplicate/watch, coordination-first, already-covered, or low-clarity question.
- Active refresh found only the `claude-code#63051` duplicate-label transition among tracked lanes; no material transition for `claude-peers-mcp#64`, `CLIProxyAPI#3594`, `mcp/python-sdk#2705`, `opencode#29705`, `mem0#5284`, `pydantic-ai#5670`, `vercel/ai#15665/#15652`, or `gemini-cli#27518`.
- Duplicate/race kept `claude-peers-mcp#64` as `CANDIDATE`, `CLIProxyAPI#3594` as broad `LEAD / possible COMMENT-FIRST later`, and fresh Claude/Gemini/opencode items as `WATCH/NO-GO`.
- Process gates rejected upstream action because mandatory skills are unavailable, runner `#10` is unavailable, CT216 is Hermes-only, and no fresh lane has clean parent gates plus actionable evidence.
- Actionability keeps `claude-peers-mcp#64` as the best compact source-level `CANDIDATE-needs-runner-validation`; runner backlog order remains `vercel/ai#15652`, `probelabs/probe#568`, `gemini-cli#27503`.
- Synthesis selected `next_status: WATCH`; no `PR-READY`.

## 3-role critique

- Factology/duplicates: approved after parent verification of `claude-code#63052`, `mcp/python-sdk#2695`, `opencode#26625`, `vercel/ai#15667`, and `koog#2079`.
- Process gates: approved internal watch/tracker update only. Upstream PR/comment count stays `0`.
- Actionability: no runner work. Next promotion still requires runner-backed validation for `claude-peers-mcp#64` or an existing runner backlog candidate.
