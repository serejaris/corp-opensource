# Spring AI toolCallId M7 watch, 2026-05-28 13:27 UTC

Контекст: bounded scouting pass после tracker-only heartbeat `2026-05-28 13:23 UTC`; tracker synthesis: [#52 comment](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4564441463). Обязательные skills `open-source-bug-scouting` / `open-source-pr-workflow` в этой среде недоступны, поэтому использован documented fallback: 6 read-only scouting roles, parent live GitHub gates, duplicate/PR/process synthesis и tracker update. Upstream actions: `0`. Runner actions: `0`.

## 6-role synthesis

- Fresh discovery поднял новый material issue-level сигнал: `spring-projects/spring-ai#6197` по tool-calling regression in `2.0.0-M7`.
- Backlog refresh сохранил `zed#57155`, `probe#568`, `claude-peers-mcp#64`, `vercel/ai#15652`, `gemini-cli#27503`, `Agent-S#195`, `spring-ai#6196`, `apify#88`, `claude-code#63129/#63133` без перехода в `PR-READY`.
- Duplicate/race scout не нашёл нового material race note: `zed#57155` остаётся candidate с worktree/archive race containment; `CodeWhale#2272` остаётся active PR race blocker.
- Process gates: Spring AI требует issue-first flow, DCO `Signed-off-by`, rebase/squash, tests and build. `#6197` ещё `status: waiting-for-triage`.
- Actionability: без dedicated runner no `PR-READY`. `#6197` требует Maven/current-main repro around OpenAI tool response construction and advisor changes.
- Scope/materiality: `#6197` заслуживает локального watch/README update как новый fresh Spring AI tool-runtime regression, separate from earlier `#6196` MCP resource-subscribe lane.

## Parent live gates

### `spring-projects/spring-ai#6197`

- Issue: [`spring-projects/spring-ai#6197`](https://github.com/spring-projects/spring-ai/issues/6197), open, created `2026-05-28T13:25:30Z`, updated `2026-05-28T13:26:36Z`.
- Labels: `status: waiting-for-triage`; assignee: none.
- Report: starting with Spring AI `2.0.0-M7`, using `OpenAiChatModel` with `ChatClient` and `@Tool`, `.defaultTools()`, or `.tools()` throws `IllegalStateException: toolCallId is required, but was not set`.
- Regression claim: same configuration worked in `2.0.0-M6` before advisor changes.
- Stack points to `OpenAiChatModel.createRequest` building `ChatCompletionToolMessageParam`, and `ToolCallAdvisor.adviseCall`.
- Environment in report: Java 25, Spring Boot 4.x, OpenAI provider.

### Duplicate / PR race

- Targeted issue search for `toolCallId required OpenAI @Tool tools 2.0.0-M7` found only `#6197`.
- Targeted PR search for `toolCallId OpenAI Tool tools` found no open PR cover.
- PRs updated after `13:23 UTC` in `spring-projects/spring-ai` did not show an obvious direct cover.
- Earlier related Spring AI lane `#6196` remains separate: MCP resource subscription capability in streamable sync WebMVC, not OpenAI tool response `toolCallId`.

### Process gate

- `spring-projects/spring-ai` uses Spring contribution process: issue-first, DCO signoff, tests/build, rebase/squash expectations.
- The issue is fresh and untriaged; no upstream comment or PR in this cycle.
- Before any upstream action: source checkout, exact current-main repro, duplicate/linked PR refresh, contribution gate, then 3-role critique.

## Decision

`next_status: CANDIDATE`

`spring-ai#6197` is a compact, high-fit tool-runtime regression candidate. It is not `COMMENT-FIRST` or `PR-READY` yet: the report has useful steps and stack trace, but we need a runner-backed or source-backed current-main repro that proves the missing `toolCallId` path in `OpenAiChatModel` / advisor handling and checks the fix against `M7` behavior.

Runner status unchanged: `corp-opensource-runner` is still unavailable via `#10`; CT216 remains Hermes-only and was not used.
