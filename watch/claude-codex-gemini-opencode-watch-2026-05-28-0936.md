# Claude/Codex/Gemini/opencode watch, 2026-05-28 09:36 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562672396)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this used the documented fallback: parent live GitHub gates, reused 6 read-only subagent roles, and focused duplicate/PR checks before status selection.

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Runner gate: dedicated `corp-opensource-runner` is still unavailable via [#10](https://github.com/serejaris/corp-opensource/issues/10). CT216 remains Hermes-only and was not used.

## Material deltas

- `anthropics/claude-code#63054`: closed at `2026-05-28T09:24:52Z`; earlier OSC52/tmux regression watch is superseded by narrower `#63061`. Status: `NO-GO / superseded`.
- `anthropics/claude-code#63061`: fresh open issue for `claude agents` OSC52 clipboard emission broken in tmux, labels `bug`, `has repro`, `regression`, `area:tui`, `area:agent-view`, `platform:wsl`, `platform:linux`. Status: product `WATCH`; internally candidate-shaped but not upstream-action-ready.
- `anthropics/claude-code#63055`: fresh open RemoteTrigger circular validator bug, labels `bug`, `has repro`, `api:anthropic`, `area:routines`; strong repro but likely product/API-owned. Status: `WATCH`.
- `openai/codex#24885`: fresh app report, server becomes unreachable after multiple rounds on macOS Codex App; weak repro and app/backend diagnostics lane. Status: `WATCH`.
- `google-gemini/gemini-cli#27519`: fresh EBADF resize crash report, duplicate bot links `#27518/#27510/#27373/#27501/#27499/#27516/#21835`. Status: `NO-GO / duplicate-cluster`.
- `sst/opencode#29713`: fresh maintainer-owned ACP-next warm switch performance PR, open, non-draft, `MERGEABLE`, checks present. Status: `WATCH / already-covered`.

Carry-forward unchanged:

- `louislva/claude-peers-mcp#64` remains the best compact source-level `CANDIDATE-needs-runner-validation`.
- `vercel/ai#15652`, `probelabs/probe#568`, and `google-gemini/gemini-cli#27503` remain runner-backed candidates but have no fresh state transition.
- `router-for-me/CLIProxyAPI#3594` remains an uncovered but protocol-heavy `LEAD`.
- `sst/opencode#29708` remains assigned/race-gated `LEAD/WATCH`; `#29710/#29712` remain active PR covers; `mcp/typescript-sdk#2163` remains an open PR cover; `langchain#37723` remains contributor-intent race.

## Parent live gates

`claude-code#63061`:

- State: open.
- Labels: `bug`, `has repro`, `platform:linux`, `area:tui`, `regression`, `platform:wsl`, `area:agent-view`.
- Body: regression on `2.1.153`, prior latest build worked; `claude agents` selection layer emits OSC52 directly, while sibling plain `claude` / shell paths still copy.
- Open PR list had no obvious exact cover.
- Decision: product `WATCH`; no upstream action without public patch surface, repeat duplicate/PR search, and runner/source evidence.

`claude-code#63055`:

- State: open.
- Labels: `bug`, `has repro`, `platform:macos`, `api:anthropic`, `area:routines`.
- Body: `RemoteTrigger` create/update fail because omitting `event_type` says required and including it says unknown field.
- Decision: `WATCH`; likely API/product-owned surface.

`codex#24885`:

- State: open, label `app`, no comments.
- Body: Codex App macOS `26.519.81530`; after multiple conversation rounds server becomes unreachable and reconnects five times, while a new session works.
- Open PR list had no obvious exact cover.
- Decision: `WATCH`; weak repro and backend/app diagnostics.

`gemini-cli#27519`:

- State: open, labels `status/need-triage`, `area/core`, `status/possible-duplicate`.
- Bot duplicate list: `#27518/#27510/#27373/#27501/#27499/#27516/#21835`.
- Existing adjacent PRs include prior PTY resize hardening/fixes, not a new clean lane.
- Decision: `NO-GO / duplicate-cluster`.

`opencode#29713`:

- State: open PR, non-draft, `MERGEABLE`.
- Scope: ACP-next warm switches, per-session directory snapshot caching, call-count and 100ms subprocess checks.
- Decision: `WATCH / already-covered`.

## 6-role synthesis

- Fresh discovery selected `claude-code#63055` and `#63061` as candidate-shaped product issues, and demoted `codex#24885`, `gemini-cli#27519`, and `opencode#29713` to watch/no-go.
- Active refresh found `claude-code#63054` closed and superseded by `#63061`, plus new `opencode#29713`; core runner candidates had no transition.
- Duplicate/race kept `claude-code#63061` as possible `CANDIDATE/COMMENT-FIRST` in isolation, but flagged adjacent tmux/OSC52 history and missing PR/source gates; `gemini-cli#27519` is duplicate-cluster; `opencode#29713` and `mcp/typescript-sdk#2163` are covered PRs; `langchain#37723` is contributor-race.
- Process gates rejected `COMMENT-FIRST` and `PR-READY`: mandatory skills unavailable, runner unavailable, CT216 Hermes-only, no current-main repro/regression card, and no complete product/source gate for Claude/Codex items.
- Actionability keeps `claude-peers-mcp#64` as the top compact source-level candidate once runner validation is available.
- Synthesis selected `next_status: WATCH`.

## 3-role critique

- Factology/duplicates: approved internal watch update only; `#63061` replaces `#63054`, `#27519` is duplicate-heavy, and `#29713/#2163` are PR-covered.
- Process gates: upstream comment/PR count stays `0`.
- Actionability: no runner work. Next promotion still requires runner-backed validation for `claude-peers-mcp#64` or another source-level lane with full duplicate/process gates.
