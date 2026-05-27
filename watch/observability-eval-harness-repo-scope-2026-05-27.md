# Observability / eval harness repo-scope watch, 2026-05-27

Tracker: [#76](https://github.com/serejaris/corp-opensource/issues/76)

Scope: `langfuse/langfuse`, `comet-ml/opik`, `confident-ai/deepeval`, `langchain-ai/openevals`, `huggingface/smolagents`.

Обязательный skill `open-source-bug-scouting` в текущей сессии недоступен; использован fallback через live GitHub/`gh` gates и 6-subagent scouting. Upstream не трогался: 0 upstream comments, 0 upstream PR.

## Decision

`next_status: WATCH`

Ни один repo/issue lane не повышен до `CANDIDATE`, `COMMENT-FIRST` или `PR-READY`: нет одного выбранного issue с полными live gates, clean duplicate/PR search, current-main runner repro и regression card.

## Parent live gates

| Repo | Live signal | Fresh lanes | Process/test gates |
|---|---|---|---|
| `langfuse/langfuse` | `28076` stars, `328` issues / `320` PRs, pushed `2026-05-27`, latest `v3.175.0`, health `75` | Redis Sentinel `#13880`, scores/evals `#13872/#13823`, dataset race `#13829`, MCP/API ordering `#13828/#13812`, usage accounting `#13811` | Core MIT, EE dirs separate; significant changes should start with issue; CLA; Node 24, pnpm 11.1.3, Docker/Postgres/Redis/ClickHouse/S3 for full repro |
| `comet-ml/opik` | `19384` stars, `86` issues / `76` PRs, pushed `2026-05-27`, latest `2.0.49`, health `62` | Online eval/filter `#6619/#6344`, ClickHouse/TiDB deploy `#6417`, trace UI `#6148/#5934` | Apache-2.0; CLA; tracked issue or `OPIK-*`; draft PR/template; AI-WATERMARK when AI-assisted; Java 21/Maven/Docker plus SDK/frontend stacks |
| `confident-ai/deepeval` | `15744` stars, `210` issues / `62` PRs, pushed `2026-05-27`, latest `v4.0.3`, health `62` | Metric async bug `#2679`, contextual precision `#2594`, pydantic-ai/OpenAI Responses instrumentation `#2508`, OTel provider `#2497` | Apache-2.0; fork/PR workflow; Poetry; upstream CI may require internal permissions, so local pytest evidence matters |
| `langchain-ai/openevals` | `1064` stars, `8` issues / `1` PR, pushed `2026-05-19`, latest `openevals-js==0.2.0`, health `75` | Typing `#95`, deprecated JS dependency `#137`, multimodal/simulator/API-shape requests `#107/#111/#112/#113/#177` | MIT; no root CONTRIBUTING found; Python `uv`/pytest/ruff/pyright/mypy, JS Yarn 3.5.1/Vitest; issue-first discipline needed despite sparse docs |
| `huggingface/smolagents` | `27554` stars, `253` issues / `304` PRs, pushed `2026-05-26`, latest `v1.25.0`, health `87` | MCP trust `#2303/#2305`, runtime discovery `#2284`, Windows/runtime `#2232/#2197`, managed-agent tool errors `#2166` | Apache-2.0; bug reports need duplicate search, env, snippet, traceback; `pip install -e ".[dev]"`, `make quality`, `make style`, `make test` |

## 6-subagent synthesis

| Role | Finding | Status |
|---|---|---|
| Repo fit/activity | `langfuse` strongest strategic fit; `opik` and `deepeval` strong eval/observability fit; `smolagents` relevant but broader; `openevals` smaller/lower-signal | `WATCH` |
| Issue actionability | Best future lanes: `langfuse#13880`, `smolagents#2166`, `deepeval#2508`; none has full gates or repro now | `WATCH` |
| Duplicate/process | Fresh `langfuse`/`opik`/`deepeval` lanes are assigned, PR-covered, internal-ticketed, or security/process sensitive | `WATCH` |
| Community/process | `langfuse`/`opik` have CLA and stronger process gates; `deepeval`/`openevals` simpler but need local evidence; `smolagents` crowded | `WATCH` |
| Runner/repro | Runner-feasible order: `smolagents`, `deepeval`, `openevals`, `langfuse`, `opik`; service-stack repos need dedicated runner before promotion | `WATCH` |
| Final synthesis | Best next deeper cycle: `langfuse` MCP/API/eval pagination lanes, then `opik` online-eval/SDK integration lanes, only after exact competing-PR gate and repro | `WATCH` |

## 3-role critique

| Critique | Finding | Status |
|---|---|---|
| Factology/duplicates | Live gates are internally consistent. Collision risk remains high: `langfuse` MCP/API/scores lanes near active PRs, `opik#6148` has `#6541`, `deepeval#2679/#2594` are PR-covered | `WATCH` |
| Process gates | CLA, issue-first, AI disclosure, EE/generated boundaries and runner requirements block any upstream action without a single issue-specific gate | `WATCH` |
| Actionability | No upstream comment/PR is justified. Next deepening must pick one issue, redo linked PR/timeline search, run current-main repro, then fill regression card | `WATCH` |

## Runner

Dedicated `corp-opensource-runner` was not used in this repo-scope cycle because no single issue was promoted past repo-level `WATCH`. Future repro should run in runner for service stacks or runtime-sensitive bugs; CT `216` is not applicable because this is not Hermes-specific.

## Follow-up gates

1. Pick exactly one lane, preferably `smolagents#2166` for a runner-light agent-harness repro or `langfuse#13880` only if Redis Sentinel service repro is realistic.
2. Recheck issue state, labels, assignee, linked PRs/timeline and exact PR search.
3. Record repo-specific contribution and license/process gates.
4. Run current-main repro/test in dedicated runner or record why runner is unavailable.
5. Fill regression card before any `COMMENT-FIRST` or `PR-READY` move.
