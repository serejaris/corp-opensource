# Zed agent worktree archive watch, 2026-05-28 13:14 UTC

Контекст: bounded scouting pass после tracker-only heartbeat `2026-05-28 13:12 UTC`; tracker synthesis: [#52 comment](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4564357290). Обязательные skills `open-source-bug-scouting` / `open-source-pr-workflow` в этой среде недоступны, поэтому использован documented fallback: 6 read-only scouting roles, parent live GitHub gates, duplicate/PR/process synthesis и tracker update. Upstream actions: `0`. Runner actions: `0`.

## 6-role synthesis

- Fresh discovery поднял новый material lead: `zed-industries/zed#57155` про удаление manually-created git worktree при archive agent conversation. Это популярный AI/editor control-plane repo, около 84k stars, direct Paperclip-like surface.
- Backlog refresh оставил core runner lanes без перехода в `PR-READY`: `probe#568` ближе к `COMMENT-FIRST` после runner repro; `claude-peers-mcp#64`, `vercel/ai#15652`, `gemini-cli#27503`, `Agent-S#195` остаются `CANDIDATE`; `spring-ai#6196` и `apify#88` остаются `WATCH/CANDIDATE` без repro/contract gate.
- Duplicate/race scout подтвердил, что `Hmbown/CodeWhale#2272` остаётся active competing PR race по task/session/env lane; новые Codex/Claude items после `13:09Z` product-owned or duplicate-adjacent.
- Process gates: Zed принимает bugfix PRs, но требует CLA; хороший PR должен быть small, tested, one thing only. `#57155` пока только `state:needs triage`, без assignee и без `state:reproducible`.
- Actionability: без dedicated runner no `PR-READY`. Самые дешёвые runner/source-first lanes остаются `claude-peers-mcp#64`, `Agent-S#195`, затем `probe#568`; Zed требует macOS/Zed UI/source checkout gate.
- Scope/materiality: в отличие от tracker-only churn, `zed#57155` заслуживает локального watch/README update как новая high-popularity repo/issue lane.

## Parent live gates

### `zed-industries/zed#57155`

- Issue: [`zed-industries/zed#57155`](https://github.com/zed-industries/zed/issues/57155), open, created `2026-05-19T14:29:40Z`, updated `2026-05-28T13:14:45Z`.
- Labels: `state:needs triage`; assignee: none.
- Fresh signal: comment at `2026-05-28T13:14:45Z` says a terminal thread action wiped a worktree with uncommitted changes.
- Repo: public, about 83.9k stars, license metadata `Other`, default branch `main`, pushed `2026-05-28T13:06:41Z`.
- Report shape: archiving an agent conversation can switch away from the current manually-created worktree and delete the worktree directory. Expected behavior is to avoid deleting user-created worktrees.

### Duplicate / PR race

- Exact PR search for `#57155` found no open PR explicitly linked to the issue.
- Archive/worktree PR search found active adjacent PRs:
  - [`#57441`](https://github.com/zed-industries/zed/pull/57441), draft, "Recreate archived worktrees from scratch on restore".
  - [`#55703`](https://github.com/zed-industries/zed/pull/55703), draft, "Prompt before clobbering files when restoring archived worktrees".
  - [`#54936`](https://github.com/zed-industries/zed/pull/54936), open, "Honor project-local worktree directory setting".
- Important merged prior art includes [`#53991`](https://github.com/zed-industries/zed/pull/53991) "Only archive worktrees that Zed created", [`#53215`](https://github.com/zed-industries/zed/pull/53215) archive/restore wiring, and [`#53629`](https://github.com/zed-industries/zed/pull/53629) skip git worktree removal on main worktrees.
- Nearby issues show the same surface is active: `#53624` is `state:reproducible` for AI agent + git worktree with devcontainer; `#55434` is closed after "archiving all threads in a worktree permanently breaks worktree"; `#56167` covers agent checkpoint worktree scans.

### Process gate

- `CONTRIBUTING.md` says contributors must sign the Zed CLA before merge.
- The project explicitly welcomes bugfix PRs, but asks for clear scope, tests, and one concern per PR.
- Current issue label is only `state:needs triage`, not `state:reproducible`; a cold PR before source checkout and exact code-path proof is premature.
- No upstream comment or PR in this cycle.

## Decision

`next_status: CANDIDATE`

`zed#57155` is a high-fit, high-impact candidate because it crosses AI agent conversation state, git worktree lifecycle, and potential data loss. It is not `COMMENT-FIRST` or `PR-READY`: parent gates still need source checkout, exact current-main repro or code-path proof, tests, and positioning against active draft PRs around archived worktree restore/clobber behavior.

Runner status unchanged: `corp-opensource-runner` is still unavailable via `#10`; CT216 remains Hermes-only and was not used.
