# MCP TypeScript / memsearch / GSD post-12:45 watch, 2026-05-28 12:53 UTC

Контекст: очередной bounded scouting pass по популярным AI-native / Paperclip-like репозиториям после heartbeat `2026-05-28 12:45 UTC`. Обязательные skills `open-source-bug-scouting` / `open-source-pr-workflow` в этой среде недоступны, поэтому использован documented fallback: 6 read-only scouting roles, parent live GitHub gates, duplicate/PR/process synthesis и tracker update. Upstream actions: `0`. Runner actions: `0`.

## 6-role synthesis

- Fresh discovery нашёл два свежих `modelcontextprotocol/typescript-sdk` bug issues: `#2166` по `UriTemplate.match()` multi-variable path expressions и `#2165` по `AbortSignal` vs timeout error code. Оба имеют MRE/root-cause shape, но reporter уже запросил assignment.
- Active backlog refresh не нашёл post-12:45 transitions для core runner candidates: `probe#568`, `claude-peers-mcp#64`, `gemini-cli#27503`, `vercel/ai#15652`, `Agent-S#195` остаются runner/source-blocked.
- Duplicate/race guard подтвердил, что `vercel/ai#15670`, `open-design#3202`, `superset#4974`, `agent-of-empires#1549/#1571`, `opencode#29734` остаются covered/occupied; core candidates clean but not upstream-ready.
- Process gates: `corp-opensource-runner` не provisioned по `#10`; CT221 только proposed, CT216 Hermes-only. Ни один lane не может перейти в `COMMENT-FIRST` или `PR-READY`.
- Actionability ranking оставил no-runner source-first порядок: `claude-peers-mcp#64`, `Agent-S#195`, scoped `apify#917`, `vercel/ai#15652`, `gemini-cli#27503`. With runner: `probe#568` first.
- Scope/materiality нашёл missing repo-scope additions: `zilliztech/memsearch` как сильный agent-memory/context layer and `gsd-build/get-shit-done` как passive high-star Claude Code context-engineering reference.

## Parent live gates

### `modelcontextprotocol/typescript-sdk#2166`

- Issue: [`#2166`](https://github.com/modelcontextprotocol/typescript-sdk/issues/2166), open, created `2026-05-28T12:47:53Z`, label `bug`.
- Report: `UriTemplate.match()` returns `null` for RFC 6570 multi-variable path expressions like `{userId,format}`.
- Parent PR search for `2166`, `UriTemplate`, `multi-variable`, `variable path` returned no open PR cover in this pass.
- Process/race gate: reporter commented at `2026-05-28T12:48:16Z`: "I'd like to work on a fix for this — could you assign the issue to me?"
- Decision: `WATCH / assignment-first`. Do not comment or open PR unless maintainer explicitly leaves the lane open or asks for outside help.

### `modelcontextprotocol/typescript-sdk#2165`

- Issue: [`#2165`](https://github.com/modelcontextprotocol/typescript-sdk/issues/2165), open, created `2026-05-28T12:47:07Z`, label `bug`.
- Report: `AbortSignal` cancellation throws `SdkErrorCode.RequestTimeout`, making abort indistinguishable from real timeout.
- Parent PR search for `2165`, `AbortSignal`, `RequestTimeout`, `REQUEST_ABORTED` returned no open PR cover in this pass.
- Process/race gate: reporter commented at `2026-05-28T12:48:35Z` asking to be assigned.
- Decision: `WATCH / assignment-first`. This likely needs API-design confirmation, not a cold PR.

### `zilliztech/memsearch`

- Public, non-fork, MIT, about 1.9k stars, pushed `2026-05-28T10:07:03Z`.
- Description: persistent unified memory layer for AI agents, including Claude Code and Codex, backed by Markdown and Milvus.
- Open issues include `#552` OpenCode plugin install silently fails, `#546` Windows Unicode stop-hook failure, `#551/#538` collection/config bugs, and `#527` rate-limit text written as memory content.
- Open PR queue is noisy/stale-ish and includes `#543` remote upsert flush, `#541` extra project paths, plugin/docs PRs.
- Decision: repo-scope `WATCH`; no issue-level candidate yet because source/repro/duplicate gate was not run.

### `gsd-build/get-shit-done`

- Public, non-fork, MIT, about 63.8k stars, pushed `2026-05-27T14:52:49Z`, updated `2026-05-28T11:55:20Z`.
- Description: lightweight meta-prompting, context-engineering and spec-driven development system for Claude Code.
- Issues and PRs are currently empty, so there is no contribution lane.
- Decision: passive repo-scope `WATCH`; useful as ecosystem reference only.

### `pydantic/pydantic-ai#5696`

- Prior status was `WATCH / pydanty-runtime-timeout`.
- Parent live gates now show open PRs [`#5697`](https://github.com/pydantic/pydantic-ai/pull/5697) and [`#5698`](https://github.com/pydantic/pydantic-ai/pull/5698), both non-draft and `BLOCKED`, covering `InstructionPart` request-part round-trip / discriminated union.
- Decision: `WATCH / duplicate-covered`; do not spend runner time or open upstream PR/comment.

## Final decision

`next_status: WATCH`

Материальный delta есть: два fresh MCP TypeScript issues, два repo-scope additions, и downgrade `pydantic-ai#5696` to duplicate-covered watch. Issue-level PR/comment не выбран: MCP lanes assignment-first, memsearch needs source/repro, GSD has no issue surface, core backlog remains runner-blocked.
