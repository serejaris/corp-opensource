# Claude/opencode/AG-UI watch, 2026-05-28 09:16 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this used the documented fallback: parent live GitHub gates, reused 6 read-only subagent roles, and focused duplicate/PR checks before status selection.

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Runner gate: dedicated `corp-opensource-runner` is still unavailable via [#10](https://github.com/serejaris/corp-opensource/issues/10). CT216 remains Hermes-only and was not used.

## Material deltas

- `anomalyco/opencode#29707`: fresh wide-character summarized paste corruption issue, assigned to `simonklee` and duplicate-hinted against `#20986/#22786`; now also covered by open PR `#29710`, non-draft `MERGEABLE`. Status: `WATCH / covered-by-PR`.
- `anomalyco/opencode#29710`: fresh PR fixing `#29707` by expanding the visible paste placeholder from prompt text instead of slicing by extmark offsets. Includes Chinese repro, regression test, `bun test test/cli/cmd/tui/prompt-part.test.ts`, and `bun typecheck`.
- `ag-ui-protocol/ag-ui#1802`: fresh PR fixing `#1801`, adding Kotlin SDK Interrupts types / serialization for `RUN_FINISHED`; non-draft `MERGEABLE`, `REVIEW_REQUIRED`, Vercel preview authorization pending. Status: `WATCH / covered-by-PR`.
- `anomalyco/opencode#29709`: fresh maintainer PR for ACP next first-session startup profiling/config/snapshot parallelization; non-draft `MERGEABLE`, no linked issue. Status: `WATCH / already-covered`.
- `anomalyco/opencode#29708`: fresh `/init` question request rejection issue, assigned to `StarpTech` and duplicate-hinted against `#28112/#27907`; status `WATCH`.
- `anthropics/claude-code#63053`: fresh VS Code extension feature request to render submitted user messages as Markdown; product-owned enhancement watch.
- `anthropics/claude-code#63052`: duplicate-labeled TUI long-line truncation issue; status `NO-GO / duplicate`.
- `JetBrains/koog#2079`: remains low-clarity `LEAD/WATCH` question around `maxTokens` type/semantics; no post-09:08 transition.

## Parent live gates

`opencode#29707/#29710`:

- `#29707` state: open, assigned `simonklee`, duplicate bot points to `#20986/#22786`.
- `#29710` state: open PR, non-draft, `MERGEABLE`.
- PR body: fixes prompt submission corruption around `[Pasted ~N lines]` when wide characters are present; adds regression test and manual verification.
- Bot related PR hint: `#25855`, likely adjacent but not exact same placeholder slicing behavior.
- Decision: `WATCH / covered-by-PR`.

`ag-ui#1802`:

- State: open PR, non-draft, `MERGEABLE`, `REVIEW_REQUIRED`.
- Fixes `#1801`.
- Scope: Kotlin SDK Interrupts protocol types/serialization only; no high-level callback/resume helper.
- Decision: `WATCH / covered-by-PR`.

`opencode#29709`:

- State: open PR, non-draft, `MERGEABLE`.
- Author `nexxeln`; scope is ACP first-session startup timing hooks and startup path optimization.
- Decision: `WATCH / maintainer-owned PR`.

`opencode#29708`:

- State: open, assigned `StarpTech`.
- Duplicate bot links `#28112/#27907`.
- Body: `/init` question selection rejects unknown request on Windows 11 / OpenCode `1.15.11`.
- Decision: `WATCH / duplicate-risk`.

`claude-code#63053`:

- State: open, labels `enhancement`, `area:ide`, `platform:vscode`, `area:ui`.
- Body: VS Code chat should render submitted user markdown for code, code blocks, and lists.
- Decision: product `WATCH`.

`claude-code#63052`:

- State: open, labels `duplicate`, `platform:macos`, `area:tui`, `area:ui`.
- Decision: `NO-GO / duplicate`.

## 6-role synthesis

- Fresh discovery found stronger material after the parent pass: `opencode#29710` covers `#29707`, `ag-ui#1802` covers `#1801`, and `opencode#29709` is a maintainer-owned ACP performance PR.
- Active refresh found no material transition for `claude-peers-mcp#64`, `CLIProxyAPI#3594`, `mcp/python-sdk#2705`, `opencode#29705`, or `koog#2079`.
- Duplicate/race classified `claude-code#63053` as `LEAD/WATCH`, `claude-code#63052` as duplicate no-go, `opencode#29707/#29708` as duplicate/covered watch, and kept `claude-peers-mcp#64` as `CANDIDATE`.
- Process gates rejected `COMMENT-FIRST` and `PR-READY` because mandatory skills are unavailable, runner `#10` is unavailable, CT216 is Hermes-only, and fresh lanes are product-owned, assigned, duplicate-risk, or already covered.
- Actionability keeps `claude-peers-mcp#64` as the best compact source-level `CANDIDATE-needs-runner-validation`; runner backlog order remains `vercel/ai#15652`, `probelabs/probe#568`, `gemini-cli#27503`.
- Synthesis selected `next_status: WATCH`.

## 3-role critique

- Factology/duplicates: approved after parent verification of `opencode#29710`, `ag-ui#1802`, and `opencode#29709`.
- Process gates: approved internal watch/tracker update only. Upstream PR/comment count stays `0`.
- Actionability: no runner work. Next promotion still requires runner-backed validation for `claude-peers-mcp#64` or an existing runner backlog candidate.
