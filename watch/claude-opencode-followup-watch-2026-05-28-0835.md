# Claude/opencode follow-up watch, 2026-05-28 08:35 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this used the documented fallback: parent live GitHub gates, reused 6 read-only roles from the current scouting cycle, and focused duplicate/PR checks before any status change.

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Runner gate: dedicated `corp-opensource-runner` is still unavailable via [#10](https://github.com/serejaris/corp-opensource/issues/10). CT216 remains Hermes-only and was not used.

## Material delta

- `anomalyco/opencode#29699` moved from open upstream-owned PR cover to `MERGED` at the parent live gate. The PR fixed OpenAI websocket response timeouts staying active, so this lane is closed for local implementation and remains historical watch only.
- `anthropics/claude-code#63048` opened/updated after the 08:30 watch: `/code-review` can silently fall back from empty `@{upstream}...HEAD` to `main...HEAD` on long-lived branches, causing review of other people's commits; the same report also asks for human-readable output instead of raw JSON-only findings.

## Parent live gates

`opencode#29699`:

- State: `MERGED`, non-draft before merge.
- Updated: `2026-05-28T08:28:02Z`.
- Parent `gh pr view` result: `state=MERGED`, `mergeable=UNKNOWN`, no comments/review decision surfaced in the final read.
- Decision: closed as upstream-owned cover; no local work or upstream comment.

`claude-code#63048`:

- State: open.
- Created: `2026-05-28T08:29:22Z`; updated: `2026-05-28T08:30:30Z`.
- Labels: `bug`, `platform:macos`, `area:skills`.
- Assignees/comments: none at parent gate.
- Focused issue search for `code-review fallback main HEAD JSON` found `#63048` plus broad adjacent skills/core issues such as `#43952`, but no exact duplicate covering the diff-scope fallback.
- Focused PR search for the same terms returned no PR cover.

## 6-role synthesis

- Fresh discovery/factology: `claude-code#63048` is high-signal and well scoped, but it targets a bundled Claude Code skill/product behavior rather than a normal external source lane.
- Active refresh: `opencode#29699` has transitioned to merged; no follow-up action is needed.
- Duplicate/race: no exact PR cover was found for `#63048`, but broad skills issues and product ownership keep it watch-only.
- Process: upstream comment/PR is blocked because this is product-owned and no external patch path is established. Parent live gates are enough for internal watch update only.
- Actionability: no runner repro was attempted; a local repo fix is not available here. Keep `#63048` as product `WATCH`, not `COMMENT-FIRST` or `PR-READY`.
- Synthesis: record both deltas, keep prior `claude-peers-mcp#64` as the only current small source-level `CANDIDATE-needs-runner-validation`.

## 3-role critique

- Factology/duplicates: approved. The only exact `code-review` fallback search hit is `#63048`; `opencode#29699` is already merged.
- Process gates: approved for internal tracker/watch update only. No upstream action and no runner action.
- Actionability: final status is `WATCH`. `claude-code#63048` may become useful as product intel, but it is not an OSS patch candidate under current gates.
