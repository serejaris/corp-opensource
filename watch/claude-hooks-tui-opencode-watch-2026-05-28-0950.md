# Claude hooks/TUI and opencode watch, 2026-05-28 09:50 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562786429)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this used the documented fallback: parent live GitHub gates, reused 6 read-only subagent roles, and focused duplicate/PR checks before status selection.

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Runner gate: dedicated `corp-opensource-runner` is still unavailable via [#10](https://github.com/serejaris/corp-opensource/issues/10). CT216 remains Hermes-only and was not used.

## Material deltas

- `anthropics/claude-code#63065`: fresh slash command autocomplete bug, labels `bug`, `has repro`, `platform:macos`, `area:tui`; repo-local duplicate search found only adjacent docs/autocomplete issues, no exact cover. Status: product `WATCH`.
- `anthropics/claude-code#63066`: fresh PreToolUse hook `if` false-positive bug, labels `bug`, `has repro`, `platform:linux`, `area:bash`, `area:hooks`, `area:permissions`; repo-local search found adjacent hooks/permission issues, no exact cover. Status: product `WATCH`.
- `anthropics/claude-code#63067`: fresh diff panel hang/whiteout issue, labels moved to `bug`, `platform:macos`, `area:ui`, `area:desktop`; weaker repro. Status: product `WATCH`.
- `sst/opencode#29714`: duplicate bot now links `#23607/#28972/#16680`; remains assigned `Hona`. Status: `WATCH / likely-duplicate-assigned`.
- `sst/opencode#29715`: fresh assigned meta issue about low-quality AI issue replies. Status: `NO-GO`.
- `openai/codex#24887`: fresh quota/account complaint with extra comment/screenshot context. Status: `NO-GO / service-account`.

Carry-forward unchanged:

- `louislva/claude-peers-mcp#64` remains the best compact source-level `CANDIDATE-needs-runner-validation`.
- `vercel/ai#15652`, `probelabs/probe#568`, and `google-gemini/gemini-cli#27503` remain runner-backed candidates but had no transition.
- `router-for-me/CLIProxyAPI#3594` remains protocol-heavy `LEAD`.
- `sst/opencode#29710/#29712` and `modelcontextprotocol/typescript-sdk#2163` remain active PR covers; `langchain#37723` remains contributor-intent race.

## Parent live gates

`claude-code#63065`:

- State: open.
- Labels: `bug`, `has repro`, `platform:macos`, `area:tui`.
- Body: pasted absolute `/Users/...` path is treated as slash command autocomplete and may launch a skill instead of sending path text.
- Repo-local search found `#52618` as adjacent docs/absolute path context, but no exact open PR cover.
- Decision: product `WATCH`; no upstream action without source/process gates.

`claude-code#63066`:

- State: open.
- Labels: `bug`, `has repro`, `platform:linux`, `area:bash`, `area:hooks`, `area:permissions`.
- Body: `PreToolUse` `if: "Bash(git *)"` can match complex Bash commands with no `git` token; may share parser with `permissions.deny`.
- Repo-local search found adjacent hooks/permissions issues but no exact cover.
- Decision: product `WATCH`; duplicate role called it candidate/comment-first-shaped, but process/actionability gates reject upstream action.

`claude-code#63067`:

- State: open.
- Labels: `bug`, `platform:macos`, `area:ui`, `area:desktop`.
- Body: diff panel hangs/whites out on large single-line JSON or modified binary file; suggests size/line-length preview guard.
- Decision: `WATCH`; weaker repro and desktop UI surface.

`opencode#29714`:

- State: open, assigned `Hona`.
- Duplicate bot links: `#23607`, `#28972`, `#16680`.
- Decision: `WATCH / likely-duplicate-assigned`.

`opencode#29715`:

- State: open, assigned `nexxeln`.
- Body: meta discussion about removing low-quality AI issue replies.
- Decision: `NO-GO`.

`codex#24887`:

- State: open, label `bug`, one comment.
- Body/comment: quota disappeared / account context.
- Decision: `NO-GO / service-account`.

## 6-role synthesis

- Fresh discovery selected `claude-code#63066` as the strongest new candidate-shaped signal and `#63067` as a lead; active refresh added `#63065/#63066/#63067`, `opencode#29715`, and `codex#24887`.
- Duplicate/race found no exact cover for `claude-code#63065/#63066`, but classified `opencode#29714` as duplicate/assigned and `codex#24887` as account/backend watch.
- Process gates rejected `COMMENT-FIRST` and `PR-READY`: mandatory skills unavailable, runner unavailable, CT216 Hermes-only, no current-main repro/regression card, and Claude/Codex items are product-owned surfaces with unclear public patch lane.
- Actionability keeps `claude-peers-mcp#64` as the top compact source-level candidate once runner validation is available.
- Synthesis selected `next_status: WATCH`.

## 3-role critique

- Factology/duplicates: approved internal watch update only; `#63065/#63066` are duplicate-light but product-gated, `#29714` is duplicate/assignee-gated.
- Process gates: upstream comment/PR count stays `0`.
- Actionability: no runner work. Next promotion still requires runner-backed validation for `claude-peers-mcp#64` or another source-level lane with full duplicate/process gates.
