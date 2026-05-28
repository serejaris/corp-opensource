# Gemini/opencode covered watch, 2026-05-28 09:02 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this used the documented fallback: parent live GitHub gates, reused 6 read-only subagent roles, and focused duplicate/PR checks before status selection.

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Runner gate: dedicated `corp-opensource-runner` is still unavailable via [#10](https://github.com/serejaris/corp-opensource/issues/10). CT216 remains Hermes-only and was not used.

## Material deltas

- `google-gemini/gemini-cli#27518`: fresh EBADF `resizePty` crash report with plausible race analysis. It is already labeled `status/possible-duplicate` and bot-linked to the active EBADF cluster, including `#27510`, `#27501`, and canonical older `#26433`; focused PR search also found old closed PR `#20811`. Keep `WATCH / NO-GO`.
- `anomalyco/opencode#29706`: fresh OpenCode Desktop `Unexpected server error` report, assigned to `jlongster`, now `needs:compliance`. The compliance bot asks for version, repro steps, and terminal, and links possible duplicate `#29083`. Keep `WATCH / NO-GO`.
- `modelcontextprotocol/python-sdk#2705`: fresh open PR resolving streamable-HTTP/SSE test TOCTOU port races; non-draft, `MERGEABLE`, `REVIEW_REQUIRED`, with full test/lint/pyright evidence. Already covered by external PR.
- `anomalyco/opencode#29705`: fresh open PR for snapshot pathspec resolution from git subdirectories, closes `#27688`, and notes prior attempts `#27737/#28131`. It is non-draft, `MERGEABLE`, compliance satisfied after bot update. Already covered by external PR.
- `anthropics/claude-code#63051`: fresh Claude Code API `529 Overloaded` report. This is server-side/transient product watch, not an OSS patch lane.
- `anomalyco/opencode#21939`: canonical animation-control issue received the GPU spike evidence from closed `#29704`; it remains assigned to `nexxeln` and watch-only.

## Parent live gates

`gemini-cli#27518`:

- State: open, unassigned.
- Labels: `status/need-triage`, `area/core`, `status/possible-duplicate`.
- Duplicate bot list: `#27499/#27510/#27443/#27373/#20792/#10523/#15623/#27516/#11502/#15609`.
- Focused search also found active EBADF cluster `#27510`, `#26433`, `#27501`, `#27499`, `#27373`, `#27504`, `#27506`, `#27517`, and older closed `#20811`.
- Decision: `WATCH / duplicate-risk`.

`opencode#29706`:

- State: open, assigned `jlongster`.
- Label: `needs:compliance`.
- Compliance bot requires OpenCode version, repro steps, and terminal; possible duplicate `#29083`.
- Decision: `WATCH / compliance-gated`, no upstream comment.

`mcp/python-sdk#2705`:

- State: open PR, non-draft, `MERGEABLE`, `REVIEW_REQUIRED`.
- Scope: tests only, streamable HTTP/SSE TOCTOU port race in parallel pytest.
- PR body reports `1271 passed`, ruff and pyright clean, and `100.00%` coverage.
- Decision: `WATCH / already-covered`.

`opencode#29705`:

- State: open PR, non-draft, `MERGEABLE`.
- Closes `#27688`; related prior attempts `#27737/#28131`.
- Bot first flagged related PRs, then guideline bot confirmed compliance.
- Decision: `WATCH / already-covered`.

`claude-code#63051`:

- State: open, no labels/assignees/comments.
- Body reports Anthropic API `529 Overloaded` on Claude Code `2.1.145`, darwin, Apple Terminal.
- Decision: `NO-GO / product-transient`.

## 6-role synthesis

- Fresh discovery found no new `LEAD/CANDIDATE`; all post-08:55 material was duplicate-covered, compliance-gated, already PR-covered, or product-transient.
- Active refresh found no material transition for `claude-peers-mcp#64`, `CLIProxyAPI#3594`, `mem0#5284`, `pydantic-ai#5670`, `vercel/ai#15665/#15652`, `probe#568`, or `gemini-cli#27517`.
- Duplicate/race kept `claude-peers-mcp#64` as `CANDIDATE`, `CLIProxyAPI#3594` as `LEAD / possibly COMMENT-FIRST later`, and demoted fresh Gemini/opencode items to `WATCH/NO-GO`.
- Process gates rejected upstream action because mandatory skills are unavailable, runner `#10` is unavailable, CT216 is Hermes-only, and no fresh lane has clean parent gates plus actionable evidence.
- Actionability keeps `claude-peers-mcp#64` as the best compact source-level `CANDIDATE-needs-runner-validation`; runner backlog order remains `vercel/ai#15652`, `probelabs/probe#568`, `gemini-cli#27503`.
- Synthesis selected `next_status: WATCH`; no `PR-READY`.

## 3-role critique

- Factology/duplicates: approved after parent verification of Gemini duplicate cluster, opencode compliance bot, and both external PR covers.
- Process gates: approved internal watch/tracker update only. Upstream PR/comment count stays `0`.
- Actionability: no runner work. Next promotion still requires runner-backed validation for `claude-peers-mcp#64` or an existing runner backlog candidate.
