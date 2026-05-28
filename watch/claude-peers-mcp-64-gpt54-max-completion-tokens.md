# claude-peers-mcp #64 gpt-5.4 max_completion_tokens

Дата: 2026-05-28

Tracker:

- Umbrella: https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562086486

Статус: `CANDIDATE-needs-runner-validation`

Upstream actions: `0`

Runner actions: `0`

## Коротко

Fresh discovery после `2026-05-28T08:20:00Z` поднял `louislva/claude-peers-mcp#64`: https://github.com/louislva/claude-peers-mcp/issues/64

Repo: https://github.com/louislva/claude-peers-mcp

- Stars: `2,040`.
- Forks: `271`.
- License: MIT.
- Archived: no.
- Last pushed: `2026-04-26T06:47:35Z`.
- Description: "Allow all your Claude Codes to message each other ad-hoc!"

Issue reports that auto-summary silently fails for users with `OPENAI_API_KEY` set because `shared/summarize.ts` sends `max_tokens` to `gpt-5.4-nano`. GPT-5 family rejects that field and asks for `max_completion_tokens`; the code then returns `null` on `!res.ok`, so users see missing summaries with no log output.

This is Paperclip-adjacent because `claude-peers-mcp` is a multi-Claude-Code peer-discovery/MCP coordination layer, and the broken auto-summary degrades cross-session awareness.

## Parent live gates

- Issue: open.
- Created/updated: `2026-05-28T08:20:54Z`.
- Labels: none.
- Assignee: none.
- Comments: `0`.
- Upstream PR/comment в этом цикле не делались.
- Runner `#10` unavailable; no local/repo checkout test was run.

Source gate from GitHub contents:

- `shared/summarize.ts` currently hardcodes `model: "gpt-5.4-nano"`.
- Same request body uses `max_tokens: 100`.
- `if (!res.ok) { return null; }` silently swallows HTTP errors.
- `package.json` exposes `bun test`, so future runner path is small if Bun is available.

## Duplicate / race

Focused search:

- Issue search for `max_tokens max_completion_tokens gpt-5.4-nano` found only `#64`.
- PR search for `max_tokens max_completion_tokens gpt-5.4-nano` found no exact PR cover.

Open PR queue is active and broad, but no current PR appears to touch this exact OpenAI auto-summary parameter bug.

Race/process risk:

- Repo has many open PRs and active maintainer/contributor traffic, so repeat PR search is required before any action.
- Issue author says "Happy to send a PR if it'd help", which creates author-PR race risk; do not open a competing PR without a fresh gate.

## 6-role synthesis

Новые subagent threads недоступны из-за thread limit, поэтому цикл переиспользовал шесть уже перечисленных agents.

- Fresh discovery selected `claude-peers-mcp#64` as the strongest new open-source candidate after `08:20Z`, alongside lower-value product/watch signals like `claude-code#63045`.
- Active refresh found no post-cutoff transition in already-tracked PR/watch lanes.
- Duplicate/race critique found no exact cover but flagged race risk for active upstream traffic and author-offered PR.
- Process critique blocks upstream PR/comment: required skills are unavailable, runner `#10` is unavailable, and current evidence is read-only/source-gate only.
- Actionability critique keeps the top runner backlog unchanged (`vercel/ai#15652`, `probelabs/probe#568`, `google-gemini/gemini-cli#27503`), but this lane is a compact Bun/TypeScript fallback candidate.

## Decision

`next_status: CANDIDATE`

Track `louislva/claude-peers-mcp#64` as a small AI-native/MCP candidate. Do not open upstream PR/comment until repeat duplicate/PR search, contribution/process check, and runner-backed validation of `max_completion_tokens` plus ideally a regression test or logged-error behavior.
