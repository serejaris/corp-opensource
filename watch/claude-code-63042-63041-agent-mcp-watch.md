# Claude Code #63042/#63041 agent and MCP watch

Дата: 2026-05-28

Tracker:

- Umbrella: https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561973121

Статус: `WATCH`

Upstream actions: `0`

Runner actions: `0`

## Коротко

Fresh discovery после `2026-05-28T08:02:51Z` поднял несколько Claude Code issues в agent/MCP surface. Две material watch lanes:

- `anthropics/claude-code#63042`: https://github.com/anthropics/claude-code/issues/63042
- `anthropics/claude-code#63041`: https://github.com/anthropics/claude-code/issues/63041

`#63042` reports that Agent tool worktrees with `isolation: "worktree"` can inherit stale local `.git/config` `user.email` / `user.name` from a prior dispatch, causing wrong committer identity and broken signed-commit verification. The issue includes a concrete repro, production evidence, workaround, and references to broader related issues `#51596` and `#36981`.

`#63041` reports that `claude mcp get <server>` prints secret-bearing MCP env values in cleartext, leaking them into terminal scrollback, Claude Code transcripts, screen shares, and logs. The issue includes concrete repro, examples, severity framing, and workaround.

Secondary watch:

- `anthropics/claude-code#63031`: `claude agents` dispatch input does not suggest workflow slash commands, while in-session completion does. This is adjacent to fixed `#61424`, includes screenshots and a concrete repro, but remains product-owned and UI-heavy.

## Parent live gates

`#63042`:

- State: open.
- Created: `2026-05-28T08:05:27Z`; updated: `2026-05-28T08:06:24Z`.
- Labels: `enhancement`, `platform:macos`, `area:agents`.
- Assignee: none.
- Comments: `0`.

`#63041`:

- State: open.
- Created: `2026-05-28T08:04:32Z`; updated: `2026-05-28T08:05:41Z`.
- Labels: `enhancement`, `area:mcp`, `area:security`.
- Assignee: none.
- Comments: `0`.

`#63031`:

- State: open.
- Created: `2026-05-28T07:41:04Z`; updated: `2026-05-28T07:42:06Z`.
- Labels: `bug`, `platform:macos`, `area:agent-view`.
- Assignee: none.
- Comments: `0`.

Upstream PR/comment в этом цикле не делались. Claude Code is product/closed-source from our contribution perspective, so no external PR path is assumed.

## Duplicate / race

`#63042`:

- Focused issue search for `subagent worktree user.email dispatch` found only `#63042`.
- Focused PR search for `subagent worktree user.email git config` found no exact PR cover.
- Related issues: `#51596` covers stale worktree reuse by agentId prefix collision; `#36981` is a broader closed request about subagent environment isolation. Neither is an exact cover for stale local git identity in a reused worktree.

`#63041`:

- Focused issue search for `mcp get env secret mask` found `#63041` plus adjacent items. The strongest adjacent is closed `#44888` about unredacted Authorization header values in `claude mcp get` / `mcp add --header`; open `#58850` covers `${VAR}` expansion during `claude mcp remove`.
- Focused PR search for `mcp get env secret mask` found no exact PR cover.
- Duplicate risk is medium because the security/MCP secret-redaction theme has prior issues, but the command/output slice is specific.

`#63031`:

- Focused issue search for `claude agents slash command completion workflow` found `#63031` and unrelated broader issues; no exact active duplicate.
- Focused PR search found no exact PR cover.
- Related closed `#61424` is adjacent, not a proven cover for workflows in the dispatch input.

## 6-role synthesis

Новые subagent threads недоступны из-за thread limit, поэтому цикл переиспользовал шесть уже перечисленных agents.

- Fresh discovery selected `#63042` and `#63041` as the strongest new Claude Code issues after `08:00Z`, with `#63031` as a UI/workflow completion watch.
- Active-watch refresh found no material post-cutoff transition in existing PR/watch lanes.
- Duplicate/race critique found no exact PR cover for `#63042`, `#63041`, or `#63031`, but noted related prior issues and product-owned race.
- Process critique blocks upstream comment/PR: required skills are unavailable, runner `#10` is unavailable, and Claude Code does not provide a normal OSS patch lane for these product surfaces.
- Actionability critique keeps broader runner backlog led by `vercel/ai#15652`, `probelabs/probe#568`, and `google-gemini/gemini-cli#27503`, but those have no fresh live delta in this cycle.
- Synthesis status for this bounded cycle: `WATCH`.

## Decision

`next_status: WATCH`

Track `#63042`, `#63041`, and `#63031` as product-owned Claude Code watch signals. No upstream comment/PR. Re-entry only if Anthropic requests external evidence, an OSS-facing docs/source path appears, or a future live gate creates a safe comment-first opportunity with new source-backed evidence.
