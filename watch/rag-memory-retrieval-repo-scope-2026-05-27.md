# RAG / memory / retrieval infra repo-scope watch, 2026-05-27

Tracker: [#77](https://github.com/serejaris/corp-opensource/issues/77)

Scope: `run-llama/llama_index`, `deepset-ai/haystack`, `mem0ai/mem0`, `qdrant/qdrant`, `lancedb/lancedb`.

Обязательный skill `open-source-bug-scouting` в текущей сессии недоступен; использован fallback через live GitHub/`gh` gates и 6-subagent scouting. Upstream не трогался: 0 upstream comments, 0 upstream PR.

## Decision

`next_status: WATCH`

Cluster сильный для Paperclip-like strategy, но лучшие свежие bug lanes уже покрыты активными PR или требуют runner-backed repro и повторного exact duplicate gate. В этом цикле не выбран безопасный `CANDIDATE`, `COMMENT-FIRST` или `PR-READY`.

## Parent live gates

Open issue counts ниже выведены как `open_issues_count - open PR count`; GitHub Search API rate-limit был достигнут, поэтому PR count взят через REST pagination headers.

| Repo | Live signal | Fresh lanes | Process/test gates |
|---|---|---|---|
| `run-llama/llama_index` | `49706` stars / `7471` forks, Python, MIT, default `main`, pushed `2026-05-26`, latest `v0.14.22`, health `100`, `186` issues / `216` PRs | `#21750` MetadataFilter NE/NIN missing-key bug, `#21774` AgentWorkflow initial_state mutation leak, `#21740` Ollama QueryEngine streaming | `CONTRIBUTING.md`, `SECURITY.md`, PR template; package tests via `uv run -- pytest`; new integration packages not accepted; AI-assisted disclosure/verification expected |
| `deepset-ai/haystack` | `25391` stars / `2810` forks, MDX/Python repo, Apache-2.0, default `main`, pushed `2026-05-27`, latest `v2.29.0`, health `100`, `89` issues / `25` PRs | `#11418` DocumentLanguageClassifier `content=None`, `#11409` HF generator stop_words duplication, `#11405` falsy metadata drop, `#11383` evaluator NaN | CLA via cla-assistant, conventional PR title, release note, AI disclaimer for fully generated PR; `hatch run test:unit`, `hatch run test:types`, `hatch run fmt` |
| `mem0ai/mem0` | `56899` stars / `6476` forks, Python, Apache-2.0, default `main`, pushed `2026-05-27`, latest `ts-v3.0.5`, health `62`, `114` issues / `291` PRs | `#5275` OpenMemory self-hosted stacked regressions, `#5245` partial embedding memory loss, `#5224` Platform v3 user_id/agent_id storage gap, `#5220` empty search query | `CONTRIBUTING.md`, Makefile/pyproject; `make test`, `make test-py-3.10/3.11/3.12`; labels sparse, security/CLA boundaries less clear |
| `qdrant/qdrant` | `31603` stars / `2299` forks, Rust, Apache-2.0, default `master`, pushed `2026-05-27`, latest `v1.18.1`, health `87`, `433` issues / `131` PRs | `#9201` NaN/Inf ingestion and ranking, `#9192` duplicate scroll records, flaky cancellation tests `#9184/#9183/#9181` | PRs should target `dev`; Rust/system-heavy checks; strict AI rules; service/e2e needs runner, Docker/compose/MinIO/protoc/consensus context |
| `lancedb/lancedb` | `10416` stars / `888` forks, HTML/Rust/Python/TS repo, Apache-2.0, default `main`, pushed `2026-05-27`, latest `v0.30.0-beta.1`, health `50`, `556` issues / `107` PRs | `#3448` Node idle CPU spikes, `#3447` Python error pickling, `#3446` Arrow IPC List<Float64> panic, older `#3352` FTS list prefilter bug | `CONTRIBUTING.md`, Rust/Python/TS surfaces; Python `make develop`, `make test`, `make doctest`, `make typecheck`; Rust/remote/S3/wheel tests need runner |

## Duplicate gates that block promotion

| Lane | Blocking evidence |
|---|---|
| `run-llama/llama_index#21750` | Open PR [`#21785`](https://github.com/run-llama/llama_index/pull/21785) says `Fixes #21750`, adds regression coverage, and is `BLOCKED`; no duplicate PR should be opened. |
| `run-llama/llama_index#21774` | Fresh PR `#21780` targets initial_state mutation leak; adjacent `#21775` also exists per subagent gate. |
| `deepset-ai/haystack#11409` | Fresh PRs `#11413/#11414` both target stop_words duplicate replies. |
| `deepset-ai/haystack#11418` | Fresh PR `#11419` targets `DocumentLanguageClassifier` with `content=None`. |
| `mem0ai/mem0#5220` | Fresh PR `#5258` rejects empty search queries. |
| `mem0ai/mem0#5275` | Fresh PR `#5276` handles part of OpenMemory Postgres/Qdrant setup regressions. |
| `qdrant/qdrant#9201` | Fresh PR `#9202` adds vector ingestion NaN/Inf / magnitude-bound checks. |
| `lancedb/lancedb#3447` | Fresh PR `#3454` makes remote error classes pickleable. |

## 6-subagent synthesis

| Role | Finding | Status |
|---|---|---|
| Repo fit/activity | `haystack` best balance; `mem0` strongest memory fit but noisy; `llama_index` strong but saturated; `lancedb` useful binding/storage watch; `qdrant` high-bar Rust/vector core | `LEAD` |
| Issue actionability | Suggested `llama_index#21750`, but parent gate later confirmed exact PR `#21785`; other strong lanes also PR-covered | `CANDIDATE` from role, downgraded by parent gate |
| Duplicate/process | Live overlap dense across all five; no cold PR without unique repro/issue | `WATCH` |
| Community/process | Best process fit `haystack` and `lancedb`; `qdrant` requires `dev` branch and heavy Rust/infra gates; `mem0` process less clear | `CANDIDATE` for future scoped lane |
| Runner/repro | Cheapest secret-free repro: `mem0` and scoped `llama_index`; `haystack` unit/fast integration feasible; `lancedb`/`qdrant` need runner for useful system cases | `WATCH` |
| Final synthesis | Future deep-dive should start with Haystack retrieval/agent reliability after finding a less-covered regression niche | `CANDIDATE` for future lane |

## 3-role critique

| Critique | Finding | Status |
|---|---|---|
| Factology/duplicates | Parent gate is decisive: fresh best lanes are PR-covered, including exact `llama_index#21750 -> #21785` | `WATCH` |
| Process gates | Process allows continued scouting/repro, but CLA/AI disclosure/branch/runner requirements block upstream action now | `CANDIDATE` for future scoped lane |
| Actionability | Suggested holding `llama_index#21750` as candidate, but this critique missed parent-confirmed `#21785`; parent decision overrides to `WATCH` | `CANDIDATE` from role, not adopted |

## Runner

Dedicated `corp-opensource-runner` was not used because no single issue survived duplicate gates as a safe current candidate. Future repro should run in runner for Qdrant/LanceDB service or heavy Rust/Python builds; CT `216` is not applicable because this is not Hermes-specific.

## Follow-up gates

1. Prefer a future Haystack retrieval/agent reliability gap only after current PR clusters settle.
2. Recheck issue state, labels, assignee, linked PRs/timeline and exact PR overlap without relying only on GitHub Search API.
3. Record repo-specific contribution gates, including CLA, AI disclosure, branch and generated/security boundaries.
4. Run current-main repro/test in dedicated runner or document a narrow local fallback.
5. Fill regression card before any `COMMENT-FIRST` or `PR-READY` move.
