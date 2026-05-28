# MCP ecosystem repo scope, 2026-05-27

Tracker: [#75](https://github.com/serejaris/corp-opensource/issues/75)

Scope: `modelcontextprotocol/registry`, `modelcontextprotocol/inspector`, `modelcontextprotocol/servers`, `modelcontextprotocol/modelcontextprotocol`.

Live check: 2026-05-27 UTC. Upstream не трогал.

## Required skills fallback

Обязательные skills `open-source-bug-scouting` и `open-source-pr-workflow` в этой сессии недоступны как локальные skill files. Fallback: 6-role subagent scouting, parent live GitHub gates через `gh`, затем 3-role critique перед tracker/watch update.

## Parent live gates

| Repo | Live repo signal | Open queue | Process / test gate | Fresh issue / PR signal |
|---|---|---:|---|---|
| `modelcontextprotocol/registry` | Go, `6,867` stars / `830` forks, pushed `2026-05-25T11:09:28Z`, latest `v1.7.9` (`2026-05-12`) | `80` open issues / `17` open PRs | Community health `87`; do not PR `data/seed.json` to publish servers; expected pipeline is Discord/Discussions -> well-scoped Issues -> PRs; `make check`; Go `1.26`, Docker/Postgres for unit/integration slices | Fresh issues `#1307/#1306/#1305/#1304/#1303/#1302/#1289`; PRs `#1310/#1290/#1267/#1266/#1207` cover publisher/validator/package-source areas |
| `modelcontextprotocol/inspector` | TypeScript, `9,894` stars / `1,334` forks, pushed `2026-05-27T21:43:59Z`, latest `0.21.2-hotfix-3` (`2026-04-14`) | `170` open issues / `104` open PRs | Community health `87`; V2 active; V1 scope is bug fixes, MCP spec compliance, docs; tests `npm test`, `npm run test-cli`, `npm run test:e2e`, Node `>=22.7.5` | Fresh V2 issues `#1371/#1370/#1369/#1368/#1364/#1354`; PRs `#1367/#1366/#1365/#1342/#1341/#1340/#1337/#1327/#1298` cover export, CI, protocol version, OAuth, labels, list_changed, CLI metadata, subpath auth and custom headers |
| `modelcontextprotocol/servers` | TypeScript, `86,341` stars / `10,842` forks, pushed `2026-05-27T09:45:23Z`, latest `2026.1.26` (`2026-01-27`) | `293` open issues / `234` open PRs | Community health `87`; no new server implementations and no README listing PRs; existing reference-server bugfix/usability/protocol-feature improvements only; TS server tests should use Vitest | Fresh issues `#4254/#4251/#4250/#4242/#4238/#4213/#4208/#4207/#4206/#4199/#4195`; PRs `#4257/#4255/#4253/#4252/#4248/#4236/#4233/#4232/#4230/#4228/#4227/#4226/#4224` cover resources/listings, fetch/time/git/filesystem clusters |
| `modelcontextprotocol/modelcontextprotocol` | TypeScript/docs/spec, `8,242` stars / `1,552` forks, pushed `2026-05-27T18:02:09Z`, latest spec release `2025-11-25` | `130` open issues / `112` open PRs | Community health `87`; spec changes follow SEP process; AI-assisted issue/PR/comment disclosure required; schema source is `schema/draft/schema.ts`; checks include `npm run prep`, `npm run check`, docs/schema generation | Fresh issues `#2806/#2762/#2749/#2748/#2734/#2721/#2698`; PRs `#2805/#2793/#2792/#2789/#2787/#2776/#2775/#2773/#2770/#2756/#2732/#2710` cover release, SEP/spec, schema/docs clarifications |

## 6-subagent synthesis

| Role | Summary | Status |
|---|---|---|
| Repo fit / activity | `inspector` has strongest Paperclip-like MCP tooling fit; `registry` has best smaller-queue publisher/validation surface; `servers` has largest ecosystem impact but high listing noise; `modelcontextprotocol` is strategic but governance-heavy. | `WATCH` |
| Issue actionability | Potential future leads: `inspector#1368`, `servers#4250/#4208`, registry publisher/validator bugs, spec `#2734`; none is promotable because obvious registry bugs are covered, inspector/servers have nearby PRs, and spec lanes need governance. | `WATCH` |
| Duplicate / process | Registry `#1306/#1289/#1184` covered by `#1310/#1290/#1267`; inspector V2 and CLI/list refresh/OAuth/header lanes have active PRs; servers git/filesystem/fetch clusters are PR-covered; spec `#2721/#2698` covered and SEP queue hot. | `WATCH` |
| Community / contribution | Registry code PRs welcome but no `data/seed.json` publishing PRs. Inspector V2/UX requires working-group context. Servers rejects new server/listing PRs. Spec repo requires SEP/process alignment and AI disclosure. | `WATCH` |
| Runner / repro | `servers` has best secret-free test surface but highest duplicate-race; `inspector` is good for CLI/client tests and browser e2e; `registry` needs Go/Docker/Postgres for deeper tests; spec repo mostly docs/schema checks. | `WATCH` |
| Final synthesis | Promotion order for later: `registry`, then `inspector`, then `servers`, then spec repo. No upstream action without one-issue current-main repro and duplicate gate. | `WATCH` |

## 3-role critique

| Critique role | Result |
|---|---|
| Фактология / дубликаты | Не повышать ни один repo/issue до `CANDIDATE`: нет current-main repro, regression card и полного linked-PR/timeline gate. Record duplicate risks: registry `#1306`/`#1310`, `#1289`/`#1290`, `#1184`/`#1267`; inspector V2 cluster and `#1334`/`#1337`, `#1292`/`#1340/#1336`, OAuth/header PRs; servers git/filesystem/fetch clusters; spec `#2721`/`#2732`, `#2698`/`#2710`, active SEP PRs. |
| Process gates | 6-subagent scouting and parent repo gates are done. Per-issue gates are not complete. Runner note is recorded; no repro/test was run. Upstream action is process-unsafe now. |
| Actionability | Exactly one final status: `WATCH`. `COMMENT-FIRST` is premature until a single issue has state/labels/assignee/timeline/linked PR, exact PR search, current-main repro, and regression card. |

## Runner

Dedicated `corp-opensource-runner` was not used because no single issue was promoted past repo/watch scouting.

Future runner gates:

- `servers`: Node/TypeScript/Vitest for package-level reference-server regressions; dedicated runner for stdio/zombie, network/private URL, large-process and real client integration.
- `inspector`: Node `>=22.7.5`; secret-free CLI/client tests first, Playwright/browser/OAuth/app flows in runner.
- `registry`: Go `1.26`; publisher/validator unit slices can be secret-free, but `make test-unit` and integration work require Docker/Postgres runner.
- `modelcontextprotocol`: Node docs/schema checks; use runner only for heavier docs links/rendering validation.

## Decision

`next_status: WATCH`.

No upstream PR/comment. Best next deeper cycle: `modelcontextprotocol/registry` publisher/validator issue only if not PR-covered, then `modelcontextprotocol/inspector` V1 bug/spec-compliance or maintainer-aligned V2 issue. `servers` remains package-level reference-server watch after exact duplicate gate. `modelcontextprotocol` remains spec/docs/schema watch only after SEP/governance alignment and AI disclosure planning.

## Inspector heartbeat - 2026-05-28 01:45 UTC

`modelcontextprotocol/inspector#1368` was rechecked in a repo-universe fallback cycle. The issue is open, unassigned, labeled `v2`, and has no comments; targeted open PR search for disconnect/session-scoped panel state found no exact cover.

3-role critique selected `next_status: CANDIDATE` for this single lane, while keeping upstream action count `0`. Promotion requires current-main UI/state repro, a focused `App.test.tsx` or equivalent test card, V2 process awareness, and a fresh duplicate search.

## Inspector readiness heartbeat - 2026-05-28 01:08 UTC

`modelcontextprotocol/inspector#1368` remains open and unassigned with `v2` label. Exact PR search still found no clear per-call/session-scoped UI state reset on disconnect, but adjacent V2/disconnect/OAuth/session/list_changed churn means duplicate search must be repeated immediately before any PR.

Decision remains `next_status: CANDIDATE`; upstream action count `0`. Current blocker: UI/state repro and focused test card against Inspector V2.
