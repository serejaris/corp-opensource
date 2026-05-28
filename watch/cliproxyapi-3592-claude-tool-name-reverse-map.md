# CLIProxyAPI #3592 reverse map для Claude tool names

Дата: 2026-05-28

Tracker:

- Umbrella: https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561820934

Статус: `CANDIDATE`

Upstream actions: `0`

Runner actions: `0`

## Коротко

Fresh discovery после `2026-05-28T07:35:00Z` нашёл `router-for-me/CLIProxyAPI#3592`: https://github.com/router-for-me/CLIProxyAPI/issues/3592

`CLIProxyAPI` — популярный AI-native routing/proxy repo: live gate показал `35,153` stars, `5,823` forks, repo не archived.

Форма бага: Claude Code отправляет Anthropic Messages tools с TitleCase именами вроде `Glob`, но Codex-backed path возвращает streamed `tool_use.name` как lowercase `glob`. Claude Code dispatch case-sensitive и отклоняет tool как недоступный. В issue есть request-side tool schema evidence, response SSE evidence, packet-capture narrowing, предложенный per-request reverse map и тестовые сценарии для TitleCase restore, lowercase-native preservation, collision handling и streaming SSE.

## Parent live gates

- Issue: open, unassigned, unlabeled, комментариев на gate time нет.
- Created/updated: `2026-05-28T07:36:11Z`.
- Repo: `router-for-me/CLIProxyAPI`, не archived.
- Contribution/process: upstream comment/PR в этом цикле не делались.
- Runner: issue `#10` по `corp-opensource-runner` остаётся open; general runner недоступен. Local/heavy repro не запускался.

## Duplicate / Race

Exact cover в open PR list на gate time не найден.

Adjacent items:

- `#3214` — open PR про request-side/Bifrost custom-tool remapping for OAuth cloaking. Он связан с `bash/read/glob` naming, но текущий issue про response-side Anthropic streaming reverse mapping для `tool_use.name`.
- В issue перечислены старые related reports `#1741`, `#2656`, `#2737`, `#3149` и PR `#2896`; это prior-art сигналы, а не current exact cover из live gate.
- Open PR list содержит translator/reasoning/WebSocket work, но obvious response-side per-request reverse-map patch не найден.

GitHub Search API вернул `403` rate-limit во время broader repo-scoped duplicate search в `2026-05-28 07:40 UTC`, поэтому gate использовал non-search issue/PR list и targeted issue/PR views. Перед любым upstream action нужен повторный exact search.

## 6-role synthesis

Новые subagent threads были недоступны из-за thread limit, поэтому цикл переиспользовал шесть уже перечисленных agents из active scouting context:

- Fresh discovery выбрал `CLIProxyAPI#3592` как сильный popular-repo candidate и дополнительно поднял `openclaw/openclaw#87570`, `trpc-agent-go#1882` и `agent-trail#123` как более слабые watch/comment-first leads.
- Active watch refresh не нашёл material post-`07:35Z` deltas по существующим PR/watch lanes.
- Duplicate/race critique оставил Candle, Hermes, Claude и Nanobot lanes ниже upstream action; `Claude#63029` остаётся duplicate-risk, `Codex#24879` остаётся covered-risk.
- Process critique блокирует upstream PR/comment: runner недоступен, source-backed repro/test не записан.
- Actionability critique всё ещё высоко ранжирует существующие Vercel/LangGraph/Gemini/Probe/Nanobot lanes для будущей runner work, но `CLIProxyAPI#3592` — самый свежий popular-repo issue с компактной contract surface.
- Synthesis выбрал единый cycle status: `CANDIDATE`.

## Watch lanes из discovery

`openclaw/openclaw#87570`: https://github.com/openclaw/openclaw/issues/87570

- Repo очень популярный по live gate, но запрос в основном про diagnostic/docs improvement вокруг plugin tool visibility при `tools.profile: "coding"`.
- Repo automation comment появился почти сразу после filing, поэтому ждать maintainer/automation output.
- Decision: `WATCH`.

`trpc-group/trpc-agent-go#1882`: https://github.com/trpc-group/trpc-agent-go/issues/1882

- MCP tool annotations to permission metadata стратегически релевантны, но issue question/design-shaped и ссылается на свежий PR `#1867`.
- Decision: `COMMENT-FIRST only if we have source-backed evidence`; иначе `WATCH`.

`agent-trail/agent-trail#123`: https://github.com/agent-trail/agent-trail/issues/123

- Сильно Paperclip-adjacent session-trail schema discussion, но repo очень ранний и не popular target.
- Decision: `WATCH`, не current candidate.

## Decision

`next_status: CANDIDATE`

Трекать `router-for-me/CLIProxyAPI#3592` как fresh popular AI-native candidate. Не открывать upstream PR/comment до повторного duplicate search, contribution/process gate, source-level repro или focused test, и записанной в этом repo 3-subagent critique.

## Follow-up, 2026-05-28 07:55 UTC

Tracker follow-up: https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561885928

Live duplicate refresh нашёл более сильный cover/race, чем исходный gate:

- `router-for-me/CLIProxyAPI#2746`: https://github.com/router-for-me/CLIProxyAPI/pull/2746
- State: open, non-draft, `CONFLICTING`, updated `2026-05-23T21:19:59Z`.
- Title: `fix(executor): use per-request collision-aware rename maps for OAuth tool name remapping`.
- Body заявляет замену global reverse map на per-request `forwardMap`/`reverseMap`, collision handling across `tools[]`, `tool_choice`, message references, and SSE stream reverse tests including `Stream_MixedCase`.

PR не merged и конфликтует, но он достаточно близок к requested fix direction в `#3592`, чтобы `#3592` больше не был чистым cold PR candidate.

Updated decision: `next_status: WATCH / duplicate-covered-risk`.

Upstream comment/PR не делать. Re-entry только после settle `#2746` или если current main всё ещё воспроизводит `Glob -> glob` после разрешения reverse-map lane.

## Follow-up, 2026-05-28 08:05 UTC

Latest duplicate/race critique keeps `#2746` as a strong cover-risk, but not a proven exact cover. The possible gap: `#2746` may only reverse names that were actually renamed during request-side OAuth collision handling, while `#3592` asks for preserving/restoring a TitleCase client-advertised tool name when the upstream response returns lowercase `tool_use.name`.

Updated decision remains `WATCH / duplicate-covered-risk`, not `NO-GO`.

No upstream action. Re-entry only after `#2746` settles or a current-main retest shows the TitleCase client-tool path still leaks lowercase names.
