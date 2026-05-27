# ChromeDevTools MCP scouting, 2026-05-27

## Snapshot

- Время live gates: 2026-05-27 18:32-18:36 UTC.
- Upstream: [`ChromeDevTools/chrome-devtools-mcp`](https://github.com/ChromeDevTools/chrome-devtools-mcp).
- Internal tracker: [#54](https://github.com/serejaris/corp-opensource/issues/54), umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52).
- Итог цикла: `next_status: WATCH`.
- Upstream PR/comment: 0.
- Runner repro: не запускался; browser/perf/windows cases требуют dedicated `corp-opensource-runner`.
- Required `open-source-bug-scouting` skill недоступен в этой сессии; fallback: 6-subagent scouting + parent `gh` gates + 3-subagent critique.

## Parent Live Gates

- Repo активный, не archived: ~41.9k stars, ~2.7k forks.
- Latest release: `chrome-devtools-mcp-v1.1.1`, published 2026-05-27 11:49 UTC.
- Default branch: `main`; license Apache-2.0.
- Open queue at gate: 65 issues, 16 PRs.
- Contribution surface: Google CLA, conventional commits, Node `.nvmrc` v24, package engines `^20.19.0 || ^22.12.0 || >=23`.
- Test surface: `npm ci`, `npm run build`, `npm run test`, targeted `npm run test tests/...`, `npm run format`, `npm run check-format`, generated docs via `npm run gen`.

## Candidate Lanes

| Lane | Status | Gate |
|---|---|---|
| [`#2138`](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/2138) path validation refactor | internal `CANDIDATE`, repo-cycle `WATCH` | Open, maintainer-authored task, no assignee/comments/labels, no open PR hits for `path validation` / `validatePath`. Needs call-site scan, affected-tools checklist, regression/test card, fresh duplicate gate before code. |
| [`#2137`](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/2137) Windows flaky tests | `WATCH` | Maintainer task, but Linux runner does not prove Windows failure. Needs 2-3 fresh Windows CI failure links and failure class before candidate promotion. |
| [`#1921`](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1921) many-tabs browser hang/crash | `WATCH / needs-runner-repro` | High impact for real-browser agent workflows, but maintainer asked for exact repro. Needs synthetic profile, many local tabs/heavy SPA, CPU/RSS/latency measurements in runner. |
| [`#2115`](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/2115) emulate timeout/desync | `WATCH / release-followup` | Assigned to `wolfib`; linked [`#2134`](https://github.com/ChromeDevTools/chrome-devtools-mcp/pull/2134) merged 2026-05-26 and released in `v1.1.0`. Only post-release confirmation is useful. |
| [`#1775`](https://github.com/ChromeDevTools/chrome-devtools-mcp/issues/1775) evaluate script from source path | `WATCH / maintainer-direction` | Feature/API design lane with conflicting open [`#1772`](https://github.com/ChromeDevTools/chrome-devtools-mcp/pull/1772) and active original author proposal. No fresh PR/comment without maintainer ask. |

## 6-Subagent Synthesis

- Repo-fit: strong Paperclip-like target because it is official Chrome DevTools MCP for coding agents, browser control, MCP clients, CLI, slim mode, skills and performance/debug workflows.
- Bug-signal: strongest agent/runtime pain is `#1921`; freshest issue-level signal was `#2115`, but live duplicate gate later found merged/released `#2134`.
- Repro-path: browser lanes need Chrome/Chrome-for-Testing and isolated profile in `corp-opensource-runner`; `#1921` is heavy, `#2115` needs post-release recheck, eval/schema lanes need deterministic fixture/eval design.
- Patchability: `#2138` is the smallest clean internal candidate: centralize path validation around tool definition metadata and test denied path handling.
- Duplicate-race: active/merged PRs cover many obvious lanes: `#2115` via `#2134`, eval guidance cluster via several closed/open PRs, `#239/#268/#1245` via active PRs. `#2138/#2137` had no open PR hits in this pass.
- PR-readiness: no `PR-READY` target. Google CLA and full test/gen/format gates apply.

## 3-Subagent Critique

- Factology/duplicates: `WATCH` is correct; do not claim repo or issue duplicate-clean beyond the specific `#2138/#2137` search terms used.
- Process gates: internal tracker/watch update is allowed; absence of runner repro blocks `CANDIDATE -> PR-READY`, not repo-level `WATCH`.
- Actionability: promote only `#2138` to internal `CANDIDATE`; do not open upstream comment/PR now.

## Unblock Events

1. `#2138`: scan all `context.validatePath(...)` call sites and tool definitions in a fresh clone; build affected-tools checklist and targeted test plan.
2. Repeat live duplicate/search gates for `#2138` immediately before code.
3. Run `npm ci && npm run build` and targeted tests in dedicated runner or document why local-only is enough for this non-browser refactor.
4. For `#1921`, use runner with isolated Chrome profile, scalable local tab fixture, and CPU/RSS/latency metrics before any upstream comment.
5. For `#2115`, only comment if latest release/current main still reproduces original timeout/desync after `#2134`.
