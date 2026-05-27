# Cycle 12 / Repro-path scouting

Дата проверки: 2026-05-27 07:32 -03.

Фокус: найти upstream issues, где реалистичен no-secret локальный regression test: unit/fixture/offline provider mapping, без live API keys и без browser-only E2E.

Метод: 6 ролей — repo-fit, bug-signal, repro-path, patchability, duplicate-race, PR-readiness.

## Shortlist

| Rank | Repo / issue | Lane | Почему |
|---:|---|---|---|
| 1 | `anomalyco/opencode#15226` — structured output + thinking sends incompatible provider request | `candidate-bug / repro-first` | Лучший fresh ready-lane candidate: несколько user reports, низкий duplicate signal, issue-linked PR разрешён. Нельзя кодить до локального payload regression. |
| 2 | `pydantic/pydantic-ai#5387` — OnlineEvaluation skips evaluators when run raises | `candidate-bug / assignment-first` | Чистый offline test, small patch в `pydantic_evals`, понятный контракт; но Pydantic rules требуют maintainer agreement/assignment для нетривиального PR. |
| 3 | `pydantic/pydantic-ai#5606` — Cerebras reasoning history / deprecated disable_reasoning | `candidate-bug / split PR / assignment-first` | Provider mapping/profile tests без Cerebras key; лучше отделить profile/model-name regression от API-shape `clear_thinking`. |
| 4 | `pydantic/pydantic-ai#5282` — TemporalDynamicToolset drops dynamic toolset instructions | `candidate-bug / heavier fixture / assignment-first` | Контракт ясен, но test может быть тяжелее из-за Temporal harness; всё равно no-key и локально проверяемо через fake/model wrapper. |
| 5 | `deepagents#3050` — virtual CompositeBackend paths do not resolve in LocalShellBackend.execute | `WATCH / design-first` | Repro локальный, но контракт спорный: command-string rewriting может быть footgun. Нужен maintainer direction перед patch. |
| 6 | `deepagents#3573` — FilesystemBackend.ls missing path indistinguishable from empty dir | `WATCH / duplicate-race` | Отличный локальный тест, но уже есть detailed competing comment + fork branch; не открывать отдельный PR без assignment gap. |

## Regression cards

### 1. `anomalyco/opencode#15226`

- Likely test file: `packages/opencode/test/provider/transform.test.ts` or nearest request-building test that sees final provider payload.
- Command: `bun test packages/opencode/test/provider/transform.test.ts -t "structured output thinking"` after confirming repo-local test conventions.
- Expected pre-fix failure: final provider payload contains both structured-output forced tool choice (`toolChoice: "required"` / provider equivalent) and thinking-enabled options for a reasoning model, matching provider errors such as "Thinking mode does not support this tool_choice".
- Contract: when structured output is requested for a thinking-capable/reasoning model, opencode must not send an incompatible tool-choice/request-shape combination to the provider; the final wire/provider-options payload is the tested contract, not a helper return value.
- Gate: before branch, inspect current issue timeline and open PRs again; if another PR already fixes the final payload with tests, switch to duplicate/watch.

### 2. `pydantic/pydantic-ai#5387`

- Likely test file: `tests/evals/test_online_capability.py`.
- Command: `uv run pytest tests/evals/test_online_capability.py -k "online_evaluation and raises"`.
- Expected pre-fix failure: new test observes `calls == []` because `handler()` raises before `_online_internal.dispatch_async(...)`; if context is forced, `_task_run.run_task()` may expose `UnboundLocalError` for duration on the error path.
- Contract: `OnlineEvaluation.wrap_run` must dispatch sampled evaluators with `output=None` and populated span/metrics context when the wrapped run raises, then re-raise the original exception.
- Likely patch surface: `pydantic_evals/pydantic_evals/online_capability.py` plus `_task_run.run_task()` duration handling.

### 3. `pydantic/pydantic-ai#5606`

- Likely test files: `tests/providers/test_cerebras.py`, `tests/models/test_cerebras.py`.
- Command: `uv run pytest tests/providers/test_cerebras.py tests/models/test_cerebras.py -k "cerebras and (zai or thinking or disable_reasoning)"`.
- Expected pre-fix failure: `CerebrasProvider.model_profile("zai-glm-4.7")` does not force `openai_chat_send_back_thinking_parts == "tags"`; `cerebras_disable_reasoning=True` maps to deprecated `extra_body["disable_reasoning"]` instead of standard `reasoning_effort="none"`; latest model names may still include stale GLM version.
- Contract: Cerebras GLM/Qwen reasoning-capable models replay prior thinking using `<think>...</think>` tags; disabling reasoning uses the supported OpenAI-compatible field; latest model-name aliases match current Cerebras GLM.
- Likely patch surface: `pydantic_ai_slim/pydantic_ai/providers/cerebras.py`, `pydantic_ai_slim/pydantic_ai/models/cerebras.py` or model settings/profile helpers.

### 4. `pydantic/pydantic-ai#5282`

- Likely test file: `tests/test_temporal.py`.
- Command: `uv run pytest tests/test_temporal.py -k "dynamic_toolset and instructions"`.
- Expected pre-fix failure: captured `ModelRequestParameters.instruction_parts` for a `TemporalAgent` run lacks sentinel text returned by the resolved dynamic toolset's `get_instructions()`, while normal agent execution includes it.
- Contract: `TemporalDynamicToolset` must preserve normal dynamic toolset instruction semantics by resolving `get_instructions()` through a Temporal activity, not inside deterministic workflow code.
- Likely patch surface: `pydantic_ai_slim/pydantic_ai/durable_exec/temporal/_dynamic_toolset.py` plus activity registration/serialization tests.

### 5. `langchain-ai/deepagents#3050`

- Likely test file: `libs/deepagents/tests/unit_tests/backends/test_composite_backend.py`.
- Command: `cd libs/deepagents && uv run pytest tests/unit_tests/backends/test_composite_backend.py -k "execute and route"`.
- Expected pre-fix failure: `CompositeBackend.execute("python /common/skills/foo.py")` delegates to default `LocalShellBackend` with literal `/common/...`, so file ops resolve the virtual path but shell execution cannot.
- Contract to clarify: agent-facing virtual paths either must be translated before shell execution, or the model-facing tool contract must make the virtual-vs-host namespace split explicit and testable.
- Lane note: design-first. Do not patch command rewriting without maintainer direction.

### 6. `langchain-ai/deepagents#3573`

- Likely test file: `libs/deepagents/tests/unit_tests/backends/test_filesystem_backend.py`.
- Command: `cd libs/deepagents && uv run pytest tests/unit_tests/backends/test_filesystem_backend.py -k "ls and missing"`.
- Expected pre-fix failure: missing path and existing non-directory both return `LsResult(error=None, entries=[])`, indistinguishable from an empty directory.
- Contract: `ls(empty_dir)` succeeds with empty entries; `ls(missing_path)` returns structured `Path not found`; `ls(file_path)` returns structured `Not a directory`.
- Lane note: no fresh PR unless assignment/duplicate gap opens; competing branch/comment already exists.

## Duplicate-race findings

- `pydantic-ai#5671` covered by open PR `#5681`.
- `pydantic-ai#5679` covered by open PR `#5680`.
- `pydantic-ai#5666` covered by open PR `#5678`.
- `pydantic-ai#5672` covered by open PR `#5674`.
- `pydantic-ai#5653` covered by open PR `#5654`.
- `deepagents#3564` covered by open PR `#3617`.
- `deepagents#3573` has a detailed external fork branch comment; treat as duplicate-race watch.
- `browser-use` sampled issues mostly require live browser/CDP/cloud/provider behavior or are already covered by prior PR clusters; no Cycle 12 no-secret deterministic candidate selected.

## Six-role synthesis

- Repo-fit: strongest fit remains `pydantic-ai` and `deepagents`; both match agent/provider/runtime harness work and have unit-test surfaces.
- Bug-signal: `pydantic-ai#5387`, `#5606`, `#5282` are real user-facing runtime contracts, not cosmetic docs.
- Repro-path: best is `#5387`; `#5606` is offline profile/settings mapping; `#5282` is local but may need Temporal test harness.
- Patchability: `#5387` and the profile half of `#5606` are small; `#5282` requires activity plumbing; `deepagents#3050` is design-sensitive.
- Duplicate-race: many fresh Pydantic bugs are already occupied; do not duplicate #5671/#5672/#5679/#5666/#5653. DeepAgents has assignment/auto-close guard and active competing branches.
- PR-readiness: choose `opencode#15226` first if local payload regression is cheap and duplicate gate remains clean; `pydantic-ai#5387` is the best pure repro but assignment-first; split `#5606`; comment/design-first on `deepagents#3050`.

## Duplicate-race addendum - 2026-05-27 07:10 -03

Fresh check used issue timeline/cross-reference API, direct PR views for known linked PRs, local watch-note text search, and web text search. GitHub Search API was still rate-limited, so treat "no direct PR found" as bounded by timeline/web/latest-known PR evidence, not as a full GitHub Search guarantee.

### Waiting / comment-first recheck

| Upstream | Fresh signal | Duplicate-race decision |
|---|---|---|
| [aaif-goose/goose#9136](https://github.com/aaif-goose/goose/issues/9136) | Still open, unassigned. Latest upstream signal is our 2026-05-27 assignment/implementation offer. Timeline shows old fork commit references by `enilsen16`, but no linked upstream PR. | `COMMENT-FIRST / low duplicate, assignment-gated`. Do not code until maintainer replies because the comment-first offer is now the newest signal. |
| [aaif-goose/goose#9332](https://github.com/aaif-goose/goose/issues/9332) | Still open, unassigned. Timeline only shows internal cross-reference plus our direction-ask comment. | `COMMENT-FIRST / low duplicate, design-sensitive`. Needs maintainer direction or Linux runner repro before PR. |
| [modelcontextprotocol/python-sdk#2687](https://github.com/modelcontextprotocol/python-sdk/issues/2687) | Still open, unassigned. Timeline shows original commenter, our reproducible failure/PR-offer comment, and internal cross-reference. No linked fix PR in issue timeline. | `COMMENT-FIRST / low duplicate, maintainer-gated`. Small patch surface, but repo rules require maintainer confirmation/assignment. |
| [microsoft/playwright-mcp#1635](https://github.com/microsoft/playwright-mcp/issues/1635) | Still open. Reporter added direct Playwright launch data; our regression-offer comment is latest. No linked PR in timeline. | `CANDIDATE / low duplicate, owner-repo gated`. Next step is maintainer confirmation whether fix belongs in `microsoft/playwright` MCP suite. |
| [OpenHands/OpenHands#14563](https://github.com/OpenHands/OpenHands/issues/14563) | Still open. Maintainer separated host-global skills from project-local skills; our Docker visibility comment is followed by a broader discoverability comment from another user. | `WATCH / design-race`. Not a no-duplicate PR lane; needs maintainer scope on container mount/config behavior. |
| [openai/codex#24725](https://github.com/openai/codex/issues/24725) | Still open, no comments. Timeline only has internal tracker cross-reference. | `WATCH / access-sensitive`. Regression card is clean, but upstream PR creation is restricted. |
| [openai/codex#23131](https://github.com/openai/codex/issues/23131) | Still open. Reporter rechecked latest `0.134.0` / current `main` on 2026-05-27 and says unresolved. Timeline cross-references [openai/codex#22470](https://github.com/openai/codex/issues/22470), not a fix PR. | `NO FRESH PR`. Reporter already supplied patch branch and cannot open PR because upstream restricts external PRs; useful action is maintainer mirror/apply watch. |

### Low-duplicate candidates

| Candidate | Why still promising | Gate before PR |
|---|---|---|
| [anomalyco/opencode#15226](https://github.com/anomalyco/opencode/issues/15226) | Best fresh candidate from cycle 12: provider request-shape bug with multiple user reports and no known open fix PR from latest bounded checks. | Reproduce locally without real provider if possible; rerun duplicate scan immediately before branch. |
| [microsoft/playwright-mcp#1635](https://github.com/microsoft/playwright-mcp/issues/1635) | Low linked-PR pressure and good browser/MCP boundary fit. | Maintainer target-repo confirmation; likely Playwright monorepo regression. |
| [aaif-goose/goose#9136](https://github.com/aaif-goose/goose/issues/9136) | Small Rust patch surface and no linked upstream PR in timeline. | Wait for assignment after our offer, because a fresh unsolicited PR would race our own comment-first process. |
| [modelcontextprotocol/python-sdk#2687](https://github.com/modelcontextprotocol/python-sdk/issues/2687) | Repro is clean and no linked fix PR in timeline. | Wait maintainer confirmation/assignment per repo rules. |

### No-go duplicate traps

- [pydantic/pydantic-ai#5671](https://github.com/pydantic/pydantic-ai/issues/5671): covered by open, mergeable, green [#5681](https://github.com/pydantic/pydantic-ai/pull/5681).
- [cline/cline#10737](https://github.com/cline/cline/issues/10737): real bug, but duplicate cluster remains active: [#8589](https://github.com/cline/cline/pull/8589) open/mergeable but likely incomplete, [#9542](https://github.com/cline/cline/pull/9542) conflicting, [#9951](https://github.com/cline/cline/pull/9951) conflicting.
- [CopilotKit/CopilotKit#4911](https://github.com/CopilotKit/CopilotKit/issues/4911): old [#4914](https://github.com/CopilotKit/CopilotKit/pull/4914) is closed; our [#5035](https://github.com/CopilotKit/CopilotKit/pull/5035) is open/mergeable and waiting review/fork preview authorization.
- [langchain-ai/deepagents#3587](https://github.com/langchain-ai/deepagents/issues/3587): our [#3616](https://github.com/langchain-ai/deepagents/pull/3616) was auto-closed by assignment guard; no second PR until maintainer responds.
- [anomalyco/opencode#29428](https://github.com/anomalyco/opencode/issues/29428): [#29474](https://github.com/anomalyco/opencode/pull/29474) attempted the likely LiteLLM/Bedrock noop-tool fix and was closed unmerged; ask direction, do not re-add same patch.
- [anomalyco/opencode#29525](https://github.com/anomalyco/opencode/issues/29525): our [#29530](https://github.com/anomalyco/opencode/pull/29530) is open/mergeable with green checks.
- [trycua/cua#1725](https://github.com/trycua/cua/issues/1725): source audit suggests current `main` may already cover the missing `click.png` path via #1718/#1720; no PR without Windows smoke failure.
- [openai/codex#24704](https://github.com/openai/codex/issues/24704): reporter supplied a reference implementation; older upstream PRs [#17248](https://github.com/openai/codex/pull/17248) and [#20909](https://github.com/openai/codex/pull/20909) were closed. Watch, do not duplicate.
- [openai/codex#23131](https://github.com/openai/codex/issues/23131): already has reporter patch branch and restricted PR path; no fresh external PR lane.
