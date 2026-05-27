# Low-code agent workflow builder repo-scope watch, 2026-05-27

Tracker: [#78](https://github.com/serejaris/corp-opensource/issues/78)

Scope: `langgenius/dify`, `FlowiseAI/Flowise`, `langflow-ai/langflow`, `n8n-io/n8n`, `activepieces/activepieces`.

Обязательный skill `open-source-bug-scouting` в текущей сессии недоступен; использован fallback через live GitHub/`gh` gates и 6-subagent scouting. Upstream не трогался: 0 upstream comments, 0 upstream PR.

## Decision

`next_status: WATCH`

Cluster сильный для Paperclip-like control-plane strategy: visual agents, workflow execution, credentials, tool nodes, memory/RAG nodes, provider adapters, streaming/UI state. Но лучшие свежие lanes уже exact-covered by open PRs, team/Linear/process-gated, или требуют runner-backed repro. Без одного незанятого issue и regression card upstream action не оправдан.

## Parent live gates

Open issue counts ниже выведены как `open_issues_count - open PR count`; PR count взят через REST pagination headers.

| Repo | Live signal | Fresh lanes | Process/test gates |
|---|---|---|---|
| `langgenius/dify` | `142899` stars / `22462` forks, TypeScript, `NOASSERTION` / modified Apache-style license, default `main`, pushed `2026-05-27`, latest `1.14.2`, health `87`, `302` issues / `512` PRs | `#36717` memory window empty output, `#36725` provider refresh CPU storm, `#36721` Vertex AI multimodal cost inflation, `#36736` reverse invocation EndUser duplication | Issue-first / `Fixes #...` / assignment discipline; backend `make lint`, `make type-check`, `make test`; frontend `pnpm -C web test`; generated/OpenAPI and security-sensitive zones need maintainer context |
| `FlowiseAI/Flowise` | `53123` stars / `24417` forks, TypeScript, `NOASSERTION` with Apache core / commercial enterprise paths per process role, default `main`, pushed `2026-05-26`, latest `flowise@3.1.2`, health `75`, `652` issues / `229` PRs | `#6428` route contract bug, `#6411` Azure latest models, `#6405` document store preview hang, `#6387` markdown Agentflows V2 | CONTRIBUTING simple, no explicit CLA found; `pnpm test`, package-level Jest, `pnpm lint`; avoid enterprise paths and API-contract regressions |
| `langflow-ai/langflow` | `148820` stars / `9129` forks, Python, MIT, default `main`, pushed `2026-05-27`, latest `v1.9.4`, health `75`, `238` issues / `699` PRs | `#13264` Knowledge Base component content, `#13231` Composio Gmail tool type error, `#13276` OpenAI Responses timeout, public security issue `#13336` | PR branch must target active `release-X.Y.Z`, not `main`; `make unit_tests`, integration/frontend checks; security via HackerOne/IBM, no public security escalation |
| `n8n-io/n8n` | `189988` stars / `58058` forks, TypeScript, `NOASSERTION` / Sustainable Use + EE boundary, default `master`, pushed `2026-05-27`, latest `n8n@2.22.4`, health `100`, `431` issues / `1038` PRs | `#31135` AI Agent v3 tool_choice, `#31056` MCP client persistent GET, `#31180` parent workflow error propagation, `#31216` eval trigger first row only | CLA required; `.ee` commercial boundary; core/workflow changes should be discussed with n8n; many issues `status:in-linear` / `team:*`; full checks include pnpm lint/typecheck/test and Playwright |
| `activepieces/activepieces` | `22442` stars / `3709` forks, TypeScript, `NOASSERTION` with MIT core / EE boundary per process role, default `main`, pushed `2026-05-27`, latest `0.83.1`, health `87`, `313` issues / `168` PRs | `#13418` bulk export folders, `#13416` worker storage growth, `#13397` Slack action resume state, `#13383` Text Helper regex extraction | Core MIT Expat; `packages/ee` / server EE commercial boundaries; scripts include `lint-core`, `lint-pieces`, `test-unit`, `test-api`, `test:e2e`; API/runtime often needs Postgres/Redis/docker-compose |

## Duplicate gates that block promotion

| Lane | Blocking evidence |
|---|---|
| `FlowiseAI/Flowise#6428` | Open PR [`#6429`](https://github.com/FlowiseAI/Flowise/pull/6429) says `fixes : #6428`, removes ambiguous `/` routes for tools/chatmessage, and is `BLOCKED`. |
| `activepieces/activepieces#13418` | Open PR [`#13420`](https://github.com/activepieces/activepieces/pull/13420) says `Tracked in GIT-1512 / #13418`, fixes bulk export for folder flows, and is `BLOCKED`. |
| `n8n-io/n8n` AI/workflow lanes | Fresh issues are mostly `status:in-linear`, `status:team-assigned`, `team:ai` / `team:cats` / `team:nodes`; PR list is dominated by team/Linear workstreams. |
| `langflow-ai/langflow` builder/agent lanes | Active PR overlap around flow builder assistant / Agent Blocks / authz and MCP handlers; public security issues should not be used for cold PR/comment lanes. |
| `langgenius/dify` workflow lanes | High churn and issue/assignment process; current workflow/provider issues require narrow repro and exact PR search before promotion. |

## 6-subagent synthesis

| Role | Finding | Status |
|---|---|---|
| Repo fit/activity | Best balance: `Flowise`; backup: `activepieces`; `Dify`/`Langflow`/`n8n` strong but noisier or process-heavy | `LEAD` |
| Issue actionability | Suggested `Flowise#6428`, but parent gate confirmed exact PR `#6429`; other lanes are wider or process-gated | `CANDIDATE` from role, downgraded by parent gate |
| Duplicate/process | `langflow`/`n8n` have active builder/agent/internal workstreams; `activepieces` and `Flowise` need issue-specific duplicate gate | `WATCH` |
| Community/process | Softest gates: `Flowise` and `activepieces`; `Langflow` release branch gate; `Dify` issue/assignment gate; `n8n` CLA/core/EE/Linear gate | `CANDIDATE` for future scoped lane |
| Runner/repro | Cheapest secret-free repro: `Flowise` route/API/unit; `Langflow` unit; `activepieces`/`Dify` often need service stack; `n8n` only narrow SQLite/unit | `LEAD` |
| Final synthesis | Best later deep-dive: `activepieces` flow/runtime correctness or `Flowise` API/Agentflow gap after current PRs settle | `CANDIDATE` for future lane |

## 3-role critique

| Critique | Finding | Status |
|---|---|---|
| Factology/duplicates | Parent gates confirm top concrete lanes are covered: `Flowise#6428 -> #6429`, `activepieces#13418 -> #13420` | `WATCH` |
| Process gates | Continued scouting is allowed, but licenses/CLA/branch/EE/security/runner gates block upstream action now | `CANDIDATE` for future scoped lane |
| Actionability | Suggested `Flowise#6428` as future deepening, but missed parent-confirmed `#6429`; parent decision overrides to `WATCH` | `CANDIDATE` from role, not adopted |

## Runner

Dedicated `corp-opensource-runner` was not used because no single issue survived duplicate gates as a safe current candidate. Future repro should use runner for Dify/Activepieces service stacks and n8n Playwright/full install; Flowise route/unit bugs may be eligible for documented lightweight fallback if package tests are secret-free. CT `216` is not applicable because this is not Hermes-specific.

## Follow-up gates

1. Revisit `Flowise` only after `#6429` merges/closes or a distinct API/Agentflow gap appears.
2. Revisit `activepieces` flow/runtime correctness only after `#13420` settles or a non-covered priority bug has clear repro.
3. Recheck issue state, labels, assignee, linked PRs/timeline and exact PR overlap without relying only on GitHub Search API.
4. Record repo-specific contribution gates, including CLA, release branch, EE/commercial paths, AI disclosure and security boundaries.
5. Run current-main repro/test in dedicated runner or document a narrow local fallback.
6. Fill regression card before any `COMMENT-FIRST` or `PR-READY` move.
