# AI-native frameworks scouting cycle 2

Дата: 2026-05-27

Метод: 6 субагентов — repo landscape, bug-signal, duplicate-race, repro/testability, harness strategy, maintainer/process.

## Текущие active PR

| PR | Статус | Действие |
|---|---|---|
| opencode [#29530](https://github.com/anomalyco/opencode/pull/29530) | Open, mergeable, duplicate/compliance/standards checks green | Wait maintainer review |
| OpenHands SDK [#3394](https://github.com/OpenHands/software-agent-sdk/pull/3394) | Open, mergeable; maintainer suggestion applied in `efdff15b` | Re-check reviews/status checks |
| CopilotKit [#5035](https://github.com/CopilotKit/CopilotKit/pull/5035) | Open, mergeable; docs preview green, other Vercel previews need team auth | Wait maintainer review/auth |
| Cline [#11087](https://github.com/cline/cline/pull/11087) | Open, mergeable; Greptile gap addressed in `e1ecc63a1`; Quality Checks green, Ubuntu/Windows matrix pending | Monitor CI/review |
| pydantic-ai [#5678](https://github.com/pydantic/pydantic-ai/pull/5678) | Open, mergeable, CI green | Wait maintainer/bot review |
| pydantic-ai [#5680](https://github.com/pydantic/pydantic-ai/pull/5680) | Open, mergeable, CI green | Wait maintainer/bot review |
| deepagents [#3616](https://github.com/langchain-ai/deepagents/pull/3616) | Closed by assignment gate | Wait issue assignment/reopen |

## Новые repo targets

| Приоритет | Repo | Почему смотреть |
|---|---|---|
| P0 | [pydantic/pydantic-ai](https://github.com/pydantic/pydantic-ai) | Лучший test surface и maintainer activity, но уже есть наши PR |
| P0 | [OpenHands/software-agent-sdk](https://github.com/OpenHands/software-agent-sdk) | Терминал/LLM/ACP/tools прямо в Hermes/Paperclip lane; не открывать новый PR пока #3394 активен |
| P0 | [anomalyco/opencode](https://github.com/anomalyco/opencode) | Активный coding-agent runtime; strict compliance bot; текущий PR #29530 |
| P1 | [langchain-ai/deepagents](https://github.com/langchain-ai/deepagents) | Очень релевантен subagents/harness, но strict assignment gate |
| P1 | [cline/cline](https://github.com/cline/cline) | Provider/MCP/CLI boundary bugs; есть Linear linkback, но public PR gaps |
| P1 | [e2b-dev/E2B](https://github.com/e2b-dev/E2B) | Sandbox/stdio reconnect bugs для long-running workers |
| P1 | [trycua/cua](https://github.com/trycua/cua) | Computer-use harness parity; OS-specific repro risk |
| P2 | [aaif-goose/goose](https://github.com/aaif-goose/goose) | Новый Rust coding-agent watch target |
| P2 | [oraios/serena](https://github.com/oraios/serena) | MCP/code-editing toolkit, хороший Python/uv surface |
| Verify-only | [openclaw/openclaw](https://github.com/openclaw/openclaw), [Gitlawb/openclaude](https://github.com/Gitlawb/openclaude) | Близко к Hermes, но нужно проверить identity/activity quality |

## Candidate bugs

| Rank | Issue | Решение |
|---:|---|---|
| 1 | Cline [#11086](https://github.com/cline/cline/issues/11086) — CLI sends `reasoning: {exclude:true}` to OpenAI-compatible GLM endpoints | PR opened: [cline/cline#11087](https://github.com/cline/cline/pull/11087). Repro found in SDK CLI `createGatewayApiHandler.createMessage` path. Follow-up covers `thinking=false` and `thinking=true` strict endpoint cases |
| 2 | E2B [#1352](https://github.com/e2b-dev/E2B/issues/1352) — resumed sandbox `commands.connect` stops stdout after large response | Strategic but may need paid/live sandbox; track first |
| 3 | deepagents [#3568](https://github.com/langchain-ai/deepagents/issues/3568) — `read_file` prompt/schema mismatch | Small patch, but reporter has local fix and another contributor asked assignment; do not jump in |
| 4 | pydantic-ai [#5671](https://github.com/pydantic/pydantic-ai/issues/5671) — Google cached content forbidden fields | Good test surface, but competing [#5681](https://github.com/pydantic/pydantic-ai/pull/5681) exists |
| 5 | OpenHands [#14563](https://github.com/OpenHands/OpenHands/issues/14563) — global skills not loaded in CLI/Docker/WebUI | Strategic, but larger container semantics |

## Duplicate no-go

- OpenHands [#14476](https://github.com/OpenHands/OpenHands/issues/14476): covered by [#14489](https://github.com/OpenHands/OpenHands/pull/14489).
- browser-use [#4801](https://github.com/browser-use/browser-use/issues/4801), [#4877](https://github.com/browser-use/browser-use/issues/4877), [#4742](https://github.com/browser-use/browser-use/issues/4742), [#4846](https://github.com/browser-use/browser-use/issues/4846), [#4579](https://github.com/browser-use/browser-use/issues/4579): active/closed PR clusters, watch only.
- pydantic-ai [#5672](https://github.com/pydantic/pydantic-ai/issues/5672): maintainer says larger fix expected this week.
- opencode [#29498](https://github.com/anomalyco/opencode/issues/29498): possible Windows path unit bug, but needs more duplicate/repro work.

## Next

1. Re-check OpenHands SDK #3394 after follow-up push.
2. Monitor Cline [#11087](https://github.com/cline/cline/pull/11087): Ubuntu/Windows matrix and maintainer review.
3. Create internal issues for Cline #11086 and E2B #1352 if not already present.
