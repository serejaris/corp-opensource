# AI-native frameworks scouting cycle 3

Дата: 2026-05-27

Метод: 6 субагентов — repo-fit, bug-signal, repro-path, patchability, duplicate-race, PR-readiness.

## Вывод

Новый PR сейчас не открывать. Лучшие маленькие баги уже либо превращены в наши active PR, либо заняты чужими open PR с зелёным CI.

Главный урок цикла: тесты надо писать на реальной границе пользовательского контракта. Для action/schema багов это raw model/tool payload -> validation/normalization -> executed browser action, а не только helper model.

## Active PR status

| PR | Live status | Действие |
|---|---|---|
| [opencode#29530](https://github.com/anomalyco/opencode/pull/29530) | Open, mergeable, duplicate/compliance/standards green | Wait maintainer review |
| [OpenHands SDK#3394](https://github.com/OpenHands/software-agent-sdk/pull/3394) | Open, mergeable, review required | Wait maintainer review |
| [CopilotKit#5035](https://github.com/CopilotKit/CopilotKit/pull/5035) | Open, mergeable; docs preview green, Vercel previews blocked by team auth | Wait maintainer/auth |
| [cline#11087](https://github.com/cline/cline/pull/11087) | Open, mergeable; Greptile gap addressed, Ubuntu/Windows tests green | Wait maintainer review |
| [pydantic-ai#5678](https://github.com/pydantic/pydantic-ai/pull/5678) | Open, mergeable, CI green | Wait maintainer review |
| [pydantic-ai#5680](https://github.com/pydantic/pydantic-ai/pull/5680) | Open, mergeable, CI green | Wait maintainer review |
| [deepagents#3616](https://github.com/langchain-ai/deepagents/pull/3616) | Closed by assignment gate, not duplicate | Wait issue assignment/reopen |

## Fresh candidates

| Rank | Candidate | Decision |
|---:|---|---|
| 1 | [browser-use#4796](https://github.com/browser-use/browser-use/issues/4796) — `bu-2-0` emits `input_text.index: true` on auth0-style login pages | WATCH / duplicate. Internal tracker [corp-opensource#27](https://github.com/serejaris/corp-opensource/issues/27). Open PR [#4815](https://github.com/browser-use/browser-use/pull/4815) already rejects boolean `InputTextAction.index`, adds `tests/ci/test_action_validation.py`, and has green CI. No new PR. |
| 2 | [pydantic-ai#5419](https://github.com/pydantic/pydantic-ai/issues/5419) — multiple MCP servers only last `mcp_list_tools` emits result | Good future candidate, but do not start before active pydantic-ai PRs resolve. Needs fresh duplicate scan and AG-UI event regression. |
| 3 | [pydantic-ai#5217](https://github.com/pydantic/pydantic-ai/issues/5217) — MCP `isError:true` blindly becomes `ModelRetry` | Good semantic bug. Needs maintainer direction on retry vs final error semantics before PR. |
| 4 | [modelcontextprotocol/python-sdk#2539](https://github.com/modelcontextprotocol/python-sdk/issues/2539) — optional `None` fields serialized as JSON `null`, breaking Node SDK Zod | Duplicate-race risk: linked [#2544](https://github.com/modelcontextprotocol/python-sdk/pull/2544). Watch/review only until checked. |
| 5 | [modelcontextprotocol/python-sdk#2507](https://github.com/modelcontextprotocol/python-sdk/issues/2507) — cancellation does not send `notifications/cancelled` | Likely duplicate/work-in-progress via [#2514](https://github.com/modelcontextprotocol/python-sdk/pull/2514). Watch/follow-up only. |
| 6 | [opencode#29294](https://github.com/anomalyco/opencode/issues/29294) — shell tool stays running after subprocess exits / pipe FD leak | Possible next runtime candidate. Needs local repro and fresh duplicate scan. |
| 7 | [goose#9136](https://github.com/aaif-goose/goose/issues/9136) — repeated tool calls / `RepetitionInspector` effectively disabled | Possible next Rust candidate. Maintainer gave fix plan; before PR search for `RepetitionInspector` / `GOOSE_MAX_TOOL_REPETITIONS` PRs. |

## Duplicate no-go

- [browser-use#4796](https://github.com/browser-use/browser-use/issues/4796): covered by [#4815](https://github.com/browser-use/browser-use/pull/4815), mergeable, CI green.
- browser-use [#4877](https://github.com/browser-use/browser-use/issues/4877), [#4801](https://github.com/browser-use/browser-use/issues/4801), [#4846](https://github.com/browser-use/browser-use/issues/4846), [#4742](https://github.com/browser-use/browser-use/issues/4742), [#4579](https://github.com/browser-use/browser-use/issues/4579): active PR clusters already exist.
- pydantic-ai [#5671](https://github.com/pydantic/pydantic-ai/issues/5671): occupied by [#5681](https://github.com/pydantic/pydantic-ai/pull/5681).
- OpenHands [#14476](https://github.com/OpenHands/OpenHands/issues/14476), [#14499](https://github.com/OpenHands/OpenHands/issues/14499), [#14523](https://github.com/OpenHands/OpenHands/issues/14523): occupied by existing PRs.
- goose [#8970](https://github.com/aaif-goose/goose/issues/8970), [#9397](https://github.com/aaif-goose/goose/issues/9397): occupied by existing PRs.

## Lesson: action/schema boundary tests

For browser-agent action bugs, the regression should preserve the raw action payload and assert the validation/execution boundary.

Bad test shape:

- only instantiate a helper with typed Python values;
- only assert that pydantic coerces `True` to `1`;
- ignore the final element/action that receives text.

Good test shape:

- replay the raw model/tool JSON, for example `{"input_text": {"index": true, "text": "..."}}`;
- assert whether the validator rejects it or emits a structured schema warning;
- if normalization is intentionally allowed, store both `raw_action` and `validated_action`;
- assert that execution cannot silently target element `1` when the raw contract supplied boolean `true`;
- for Python/TS parity, keep the same raw fixture across both runtimes.

## Next

1. Keep monitoring active PRs before starting unrelated new PRs.
2. If a new slot opens, prefer `opencode#29294` or `goose#9136` after duplicate scan.
3. Treat `browser-use#4796` as a lesson/watch item, not our PR lane, unless maintainers ask for extra traceability tests beyond #4815.
