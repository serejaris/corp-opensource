# Hermes state.db compression watch, 2026-05-28 13:59 UTC

Контекст: bounded scouting pass после tracker heartbeat `2026-05-28T13:57:13Z`; tracker synthesis: [#52 comment](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4564794291). Обязательные skills `open-source-bug-scouting` / `open-source-pr-workflow` в этой среде недоступны, поэтому использован documented fallback: 2 read-only scouting roles из-за `agent thread limit reached`, parent live GitHub gates, duplicate/PR/actionability synthesis. Upstream actions: `0`. Runner actions: `0`.

Follow-up `2026-05-28 14:13 UTC`: upstream triaged [`#33906`](https://github.com/NousResearch/hermes-agent/issues/33906) as `type/bug`, `comp/agent`, `P2`, and marked [`#33907`](https://github.com/NousResearch/hermes-agent/issues/33907) duplicate of `#33906`; tracker synthesis: [#52 comment](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4564933516). Related PR [`#32070`](https://github.com/NousResearch/hermes-agent/pull/32070) covers `user_id` propagation for compression continuations but not the missing `state.db` row described here.

## Scouting synthesis

- Fresh discovery поднял новый Hermes-specific issue-level сигнал: `NousResearch/hermes-agent#33906`, with exact duplicate `#33907` filed seconds later.
- Backlog/race refresh сохранил `terraform-mcp-server#376/#377`, `zed#57155/#57957`, `spring-ai#6197`, `probe#568`, `mcphub#861`, `claude-peers-mcp#64`, `Agent-S#195`, `vercel/ai#15652`, `gemini-cli#27503`, `spring-ai#6196`, `apify#88` без перехода в `PR-READY`.
- Parent duplicate/race gates found no open PR cover for context-compression `state.db` registration.
- Parent actionability critique: this is Hermes-specific, so CT216 may be used only for a bounded Hermes smoke/repro if needed; no runner action was taken in this scouting cycle.

## Parent live gates

### `NousResearch/hermes-agent#33906` / `#33907`

- Issues: [`#33906`](https://github.com/NousResearch/hermes-agent/issues/33906) and [`#33907`](https://github.com/NousResearch/hermes-agent/issues/33907), both open, created `2026-05-28T13:59:24Z` / `2026-05-28T13:59:30Z`.
- Labels: none; assignees: none; comments: none at gate time.
- Repo: [`NousResearch/hermes-agent`](https://github.com/NousResearch/hermes-agent), public, MIT, about 171k stars, default branch `main`, pushed `2026-05-28T12:47:30Z`.
- Report: automatic context compression rotates `session_id` and creates a new JSON session file, but sometimes fails to write the continuation row to `state.db`. The WebUI sidebar then shows the continuation as an orphan instead of nesting it under the parent session.
- Observed example: session `20260528_104535_fdc667` exists on disk with 114 messages but has no `state.db` row, while sibling continuations were correctly registered.
- Source pointers in report: `run_agent.py:10418-10455` wraps compression split / `create_session()` in a broad `try/except` and continues on failure; `streaming.py:3598-3694` detects `agent.session_id != session_id`, updates the in-memory WebUI object and writes JSON, but does not independently ensure a `state.db` row.

### Duplicate / PR race

- `#33906` and `#33907` are exact duplicates from the same author with identical body.
- Targeted issue search for `context compression orphan state.db session` did not return a separate older open issue.
- Targeted open PR searches for `context compression state.db session orphan`, `state.db create_session compression session`, and `run_agent streaming session state.db` found no direct PR cover.
- Older `state.db` or session PRs may be adjacent, but no exact cover was found for compression continuation registration.

### Process / runner gate

- This lane is Hermes-specific, so CT216 is allowed only for bounded Hermes smoke/repro checks before the dedicated runner exists.
- No CT216/runner action was taken in this cycle: the current pass is scouting/triage only, and a repro needs a controlled compression-rotation fixture or source-level unit around `create_session()` failure and WebUI defensive registration.
- No upstream comment/PR before duplicate consolidation, source checkout, current-main repro/test card, and 3-role critique.

## Decision

`next_status: CANDIDATE-backup`

`hermes-agent#33906` is a relevant agent-session persistence candidate: it affects long-running WebUI sessions, context compression, and lineage visibility in `state.db`. It is backup rather than primary because `#33906/#33907` are duplicate-race issues, the report is currently source-reasoned rather than independently reproduced, and existing higher-priority candidates remain open.

## Follow-up triage, 2026-05-28 14:13 UTC

- `#33906` remains open and now has labels `type/bug`, `comp/agent`, `P2`; assignee: none; comments: none.
- `#33907` remains open with labels `type/bug`, `duplicate`, `comp/agent`, `P2`.
- Maintainer/collaborator comment on `#33907`: duplicate of `#33906`; related PR `#32070` fixes `user_id` propagation in compression continuation sessions.
- Parent PR gate: `#32070` is open, non-draft, touches `agent/conversation_compression.py`, and fixes missing `user_id` on continuation sessions. It is adjacent but not an exact cover for the `#33906` failure mode where a continuation JSON exists but the `state.db` row is missing.

Updated status: `CANDIDATE-backup / triaged P2`. No upstream comment/PR: still needs source checkout, duplicate consolidation, current-main repro/test card, and 3-role critique.

Runner status unchanged: `corp-opensource-runner` is still unavailable via `#10`; CT216 remains Hermes-only and was not used.
