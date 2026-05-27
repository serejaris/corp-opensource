# BloopAI/vibe-kanban scouting, 2026-05-27

Live state: parent `gh` pass на 2026-05-27 19:15-19:42 UTC.

Upstream: [`BloopAI/vibe-kanban`](https://github.com/BloopAI/vibe-kanban).
Internal tracker: [`corp-opensource#59`](https://github.com/serejaris/corp-opensource/issues/59).
Umbrella tracker: [`corp-opensource#52`](https://github.com/serejaris/corp-opensource/issues/52).

Upstream actions в этом блоке: 0 PR, 0 comments.

Final `next_status`: `WATCH`.

Это repo-level control-plane scan, а не выбранный upstream bug target. Required skills `open-source-bug-scouting` и `open-source-pr-workflow` в текущей session недоступны, поэтому использован documented fallback: 6 read-only scouting subagents, parent live gates, затем 3-subagent critique.

## Parent Live Gates

| Gate | Result |
|---|---|
| Repo state | `BloopAI/vibe-kanban`: 26549 stars, 2783 forks, default branch `main`, latest release `v0.1.44-20260424091429`, pushed `2026-04-24T09:17:34Z`, updated `2026-05-27T19:03:14Z`. |
| Queue pressure | 376 open issues, 151 open PRs. Выборка PR queue: 12 draft / 139 ready; многие PR dirty или blocked. |
| Product fit | Сильный Paperclip-like control-plane fit: task/workspace UI, coding-agent execution, Git/PR flow, MCP surface, self-hosting, executor orchestration. |
| Maintainer rhythm | Слабый для новых external PR. Recent merged PRs в основном maintainer-authored и сгруппированы на `2026-04-24`, включая sunset/export-only work. May PRs продолжают приходить, но reviews/merges редкие. |
| Process gate | README сообщает, что project is sunsetting, и просит contributors обсуждать ideas/changes с core team через GitHub Discussions или Discord до открытия PR. |
| Contribution surface | `CONTRIBUTING.md`, issue template и PR template не найдены. Code of Conduct есть. License file: Apache-2.0. |
| Test surface | Rust + pnpm monorepo. Dev path: `pnpm i`, `pnpm run dev`; checks включают `pnpm run check`, `pnpm run lint`, `cargo check --workspace`, clippy, nextest, generated type/db checks. Remote checks могут требовать private deploy key paths. |
| Runner gate | Не применялся для repo-level scan. Любой будущий issue-level candidate по sessions, agents, MCP, Docker/self-host, memory/perf или Tauri требует сначала runner/local app repro. |

## 6-Subagent Synthesis

| Role | Finding |
|---|---|
| repo-fit | Отличный control-plane reference, но пока это скорее scoped watch / fork-scouting target, а не normal PR hunting target из-за sunsetting state. |
| issue triage | Лучший lead — `#3329` (workspace cards disappear after local cleanup), потому что report содержит конкретный code path. `#3247` слабее как session-switching lead. `#3406` имеет высокий data-loss impact, но слабый repro. |
| PR / duplicate pressure | Open PR queue большой. MCP/session/workspace areas crowded. `#3349` covered by open `#3420`; `#3343` overlaps open `#3394`; memory/OOM lanes overlap `#3218/#3244/#3352`. |
| process gates | README требует prior discussion before PR; cold PR invalid. Любое eventual upstream action должно быть comment/discussion-first с repro и approval ask. |
| repro / runner | Local app stack тяжёлый. Pure frontend/static bugs можно проверять локально, но agent/session/MCP/Docker/memory/Tauri issues требуют runner или dedicated app repro. |
| PR-readiness | Нет `PR-READY` lane. Repo-level final status is `WATCH`; issue-level candidates требуют отдельные gates. |

## Leads

Это только leads, не выбранные candidates.

| Upstream | Current signal | Status |
|---|---|---|
| [`#3329`](https://github.com/BloopAI/vibe-kanban/issues/3329) | Open, no assignee/labels/comments; report называет `packages/web-core/src/features/kanban/ui/KanbanContainer.tsx` и объясняет display-layer filter после workspace cleanup. Targeted PR search не нашёл exact PR, но рядом есть workspace/kanban overlap risk. | Strong lead after repro; not duplicate-clean, not PR-ready. |
| [`#3247`](https://github.com/BloopAI/vibe-kanban/issues/3247) | Open, no assignee/labels/comments; session switching unavailable в plan approval / `AskUserQuestion` modes. Есть related broader `AskUserQuestion` support surface. | `LEAD/WATCH`; not candidate yet. |
| [`#3406`](https://github.com/BloopAI/vibe-kanban/issues/3406) | Potential data-loss report вокруг deleting workspace / git repository state, но reporter uncertainty и нет repro details. | Comment-first only after safe repro plan. |
| [`#3349`](https://github.com/BloopAI/vibe-kanban/issues/3349) | MCP backend discovery stale port / loopback mismatch. | Covered by open PR [`#3420`](https://github.com/BloopAI/vibe-kanban/pull/3420); watch only. |
| [`#3343`](https://github.com/BloopAI/vibe-kanban/issues/3343) | Long agent responses rendered twice. | Overlaps open PR [`#3394`](https://github.com/BloopAI/vibe-kanban/pull/3394); no new PR. |
| [`#3352`](https://github.com/BloopAI/vibe-kanban/issues/3352) / [`#3218`](https://github.com/BloopAI/vibe-kanban/issues/3218) | Memory/OOM / historical log replay. | Needs runner/perf repro and overlaps PR [`#3244`](https://github.com/BloopAI/vibe-kanban/pull/3244). |

## 3-Subagent Critique

| Role | Result |
|---|---|
| factology / duplicates | `WATCH` корректен. Не называть `#3329` duplicate-clean; это только strong lead. Не называть `#3247` candidate из-за broader `AskUserQuestion` overlap. |
| process gates | Cold PR/comment сейчас invalid: upstream asks for discussion before PR, repo is sunsetting, и нет issue-level repro/regression card. |
| actionability | Обновить только internal tracker/watch/repo-card. Нет upstream action и нет runner repro из этого repo-level scan. |

## Decision

Держать `BloopAI/vibe-kanban` как scoped Paperclip-like control-plane watch, а не near-term upstream PR target.

Final `next_status`: `WATCH`.

Следующий bounded gate:

1. Watch maintainer/community signal in [`#3408`](https://github.com/BloopAI/vibe-kanban/issues/3408), Discussions `#3412/#3416`, and open PR reviews/merges.
2. Если maintainer rhythm улучшится, запустить отдельный issue-level scan сначала по `#3329`.
3. Перед любым upstream action: current-main repro, duplicate PR/issue search, regression card, and 3-subagent critique.
4. Первое upstream action должно быть comment/discussion-first с repro и approval ask, не PR.
