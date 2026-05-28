# Codex/Claude product watch 2026-05-28 08:30

Дата: 2026-05-28

Tracker:

- Umbrella: https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562138734

Статус: `WATCH`

Upstream actions: `0`

Runner actions: `0`

## Коротко

Bounded refresh после `3abb386` / tracker heartbeat `2026-05-28T08:24:46Z` подтвердил, что `louislva/claude-peers-mcp#64` остаётся internal `CANDIDATE` без post-cutoff PR cover, а fresh material deltas относятся к product-owned watch lanes:

- `openai/codex#24884`: Windows Codex App WSL mode requires `danger-full-access` because sandbox blocks WSL access.
- `anthropics/claude-code#63047`: Plugin PostToolUse hooks still silently skip in Claude Desktop / Cowork.

## Parent live gates

`openai/codex#24884`: https://github.com/openai/codex/issues/24884

- State: open.
- Created/updated: `2026-05-28T08:25:07Z`.
- Labels: `app`.
- Assignee: none.
- Comments: `0`.
- Reported shape: on Windows Codex App, WSL mode cannot enumerate/use distros under `sandbox = "elevated"` or `sandbox = "unelevated"`; only `danger-full-access` works.

`anthropics/claude-code#63047`: https://github.com/anthropics/claude-code/issues/63047

- State: open.
- Created: `2026-05-28T08:26:09Z`; updated: `2026-05-28T08:27:38Z`.
- Labels: `bug`, `has repro`, `platform:windows`, `area:hooks`, `area:cowork`, `area:plugins`.
- Assignee: none.
- Comments: `0`.
- Reported shape: plugin `PostToolUse` hooks fire in Claude Code CLI but never fire in Claude Desktop / Agent Mode (Cowork), with six weeks of production telemetry evidence.

`louislva/claude-peers-mcp#64`: https://github.com/louislva/claude-peers-mcp/issues/64

- State: open.
- No comments, labels, or assignee changes after the previous candidate note.
- Focused PR search still finds no exact `max_tokens -> max_completion_tokens` cover.

## Duplicate / race

`codex#24884`:

- Focused search for `WSL App sandbox danger-full-access` found `#24884` plus older adjacent Windows sandbox issues.
- Strong adjacent: `#21470` (`Windows Codex Desktop sandbox blocks Node child_process, WSL, Schannel clients, and pip temp installs`) is broader and older; not an exact WSL-mode/danger-full-access repro, but it is the main cover-risk.
- PR search for `WSL sandbox E_ACCESSDENIED` found no current exact PR cover.
- Status impact: product/app watch only; no source/runner lane here.

`Claude#63047`:

- Issue itself is a re-file of closed/stale `#51904`.
- Related canonical thread: open `#16288`.
- Adjacent closed/duplicate reports: `#27398`, `#51281`, `#51904`.
- Focused PR search found no exact cover, but duplicate/process risk is high and surface is product-owned.

`claude-peers-mcp#64`:

- No exact PR cover after repeat search.
- Race remains: issue author explicitly offered to send a PR, and repo PR queue is active.

## 6-role synthesis

Новые subagent threads недоступны из-за thread limit, поэтому цикл переиспользовал шесть уже перечисленных agents.

- Fresh discovery highlighted `codex#24884` and `Claude#63047` as material post-cutoff signals; `oh-my-openagent#4584` and `agentdeck#9` were lower-priority watch/reference leads.
- Active refresh found `opencode#29699` updated but still open with no merge/review transition.
- Duplicate/race critique kept `claude-peers-mcp#64` as `CANDIDATE`, `codex#24884` as product watch with `#21470` cover-risk, and `Claude#63047` as duplicate-heavy watch around `#16288/#51904`.
- Process critique blocks upstream comment/PR: required skills unavailable, runner `#10` unavailable, and new material deltas are product-owned or still need runner validation.
- Actionability critique keeps the top runner backlog unchanged: `vercel/ai#15652`, `probelabs/probe#568`, `google-gemini/gemini-cli#27503`; `claude-peers-mcp#64` remains a compact fallback candidate, not `PR-READY`.

## Decision

`next_status: WATCH`

Record `codex#24884` and `Claude#63047` as fresh product watch signals. Keep `claude-peers-mcp#64` as prior internal `CANDIDATE-needs-runner-validation`; no upstream comment/PR.
