# FastMCP scouting, 2026-05-27

## Snapshot

- Время live gates: 2026-05-27 18:40 UTC.
- Upstream: [`PrefectHQ/fastmcp`](https://github.com/PrefectHQ/fastmcp).
- Internal tracker: [#55](https://github.com/serejaris/corp-opensource/issues/55), umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52).
- Итог цикла: `next_status: WATCH`.
- Upstream PR/comment: 0.
- Runner repro/test: не запускался. Причина не в недоступности runner, а в process gate: выбран `WATCH`, upstream action не планируется, `PR-READY` не достигнут, FastMCP assignment-first rule блокирует external PR без maintainer approval/assignment.
- Required `open-source-bug-scouting` skill недоступен в этой сессии; fallback: 6-subagent scouting + parent `gh` gates + 3-subagent critique.

## Parent Live Gates

- Repo активный, не archived: 25,345 stars, 2,034 forks.
- Default branch: `main`; license Apache-2.0.
- Latest stable release at gate: `v3.3.1`, published 2026-05-15 15:49 UTC. Отдельно замечен beta tag/release `v3.4.0b1` от 2026-05-23.
- Open queue at gate: 218 issues, 15 PRs.
- Fresh checkout for source/process read: `/tmp/fastmcp-scout` at `989f6f8` (`docs: add tool fingerprinting recipe for stable schema hashing (#4233)`), 2026-05-27 18:40 UTC.
- Python surface: `>=3.10`, classifiers 3.10-3.13, `.python-version` 3.12, `uv` workspace with `fastmcp_slim` and `fastmcp_remote`.
- Test surface: `uv run --frozen pytest`, `uv run --frozen ty check`, `uv run prek run --all-files`; CI covers Python 3.10 Ubuntu/Windows, 3.13 Ubuntu, lowest deps, conformance with Node 22/npx, integration, package install smoke, static checks.

## Contribution Gate

FastMCP `CONTRIBUTING.md` is the central blocker for cold PRs:

- "best contribution is a great issue";
- open issue is not invitation to PR;
- propose approach in the issue first and ask maintainer to assign;
- external PRs referencing issues not assigned to the PR author are auto-closed;
- bug fix PRs are acceptable only for simple well-scoped cases;
- enhancements need design proposal first.

Therefore this cycle records watch/comment-first lanes only. No upstream comment/PR is posted from this pass.

## Candidate Lanes

| Lane | Status | Gate |
|---|---|---|
| [`#4143`](https://github.com/PrefectHQ/fastmcp/issues/4143) `event_store.store_event` concurrent eviction | `COMMENT-FIRST-after-MRE/assignment`, cycle `WATCH` | Open, unassigned, bug/server. Concrete traceback and suggested idempotent-delete fix, but closed unmerged PRs [`#4144`](https://github.com/PrefectHQ/fastmcp/pull/4144), [`#4171`](https://github.com/PrefectHQ/fastmcp/pull/4171), [`#4226`](https://github.com/PrefectHQ/fastmcp/pull/4226) make direct PR a process replay. Any future comment must include current-main MRE, address the `#4171` stale stream-index concern, and ask for assignment before coding. |
| [`#4124`](https://github.com/PrefectHQ/fastmcp/issues/4124) proxy cancellation not forwarded upstream | `COMMENT-FIRST-after-MRE/assignment`, cycle `WATCH` | Open, unassigned, bug/server with MRE and production-impact comment. Closed unmerged [`#4145`](https://github.com/PrefectHQ/fastmcp/pull/4145) shows the same assignment/process gate. Semantics are broader than a tiny patch; needs maintainer cancellation contract before code. |
| [`#4192`](https://github.com/PrefectHQ/fastmcp/issues/4192) Windows HTTP/SSE orphan tasks | `WATCH / needs-Windows-current-main-repro` | Open, unassigned, high-priority. Maintainer could not reproduce on macOS/current main and points to possible MCP SDK or `sse-starlette` location. Needs Windows runner/VM evidence and patch-surface confirmation. |
| [`#4246`](https://github.com/PrefectHQ/fastmcp/issues/4246) OIDCProxy AsyncClient leak | `WATCH / duplicate-covered` | Covered by open maintainer/member PR [`#4248`](https://github.com/PrefectHQ/fastmcp/pull/4248). No parallel action. |
| [`#4161`](https://github.com/PrefectHQ/fastmcp/issues/4161) proxy notifications | `WATCH / duplicate-covered` | Covered by existing open PR [`#4191`](https://github.com/PrefectHQ/fastmcp/pull/4191), still review-gated. No duplicate PR. |
| [`#4205`](https://github.com/PrefectHQ/fastmcp/issues/4205) OAuth discovery / CIMD flow | `WATCH / assigned-active-PR` | Issue assigned to PR author; open PR [`#4206`](https://github.com/PrefectHQ/fastmcp/pull/4206) is under maintainer review/requested changes. No intervention. |
| [`#4154`](https://github.com/PrefectHQ/fastmcp/issues/4154) namespace ResourceLink URI rewrite | `COMMENT-FIRST-after-design`, cycle `WATCH` | Closed PR [`#4156`](https://github.com/PrefectHQ/fastmcp/pull/4156) and maintainer direction imply reusable result-transforming abstraction, not one-off wrapper. |
| [`#3715`](https://github.com/PrefectHQ/fastmcp/issues/3715) live session callback update | `WATCH` | Maintainer signal waits for public SDK API; no private attr mutation. |
| [`#3998`](https://github.com/PrefectHQ/fastmcp/issues/3998) OpenTelemetry list-method trace context | `WATCH` | Has closed unmerged PR [`#4146`](https://github.com/PrefectHQ/fastmcp/pull/4146); comment-first only if fresh evidence/design is needed. |
| [`#1197`](https://github.com/PrefectHQ/fastmcp/issues/1197) default serializer / Pydantic config | `WATCH / lower-priority` | Older and patchable-looking, but not selected over fresher assignment-gated reliability lanes. |

## 6-Subagent Synthesis

- Repo-fit: strong Paperclip-like MCP runtime target; direct client/server/tool framework surface for agents.
- Bug-signal: strongest narrow reliability leads are `#4143` and `#4124`; highest-priority environment lead is `#4192`, but it needs Windows current-main repro.
- Repro-path: `#4143/#4124` can be reproduced with secret-free local/runner tests; `#4192` needs Windows or equivalent CI evidence. No repro was run because this cycle stops at `WATCH`.
- Patchability: `#4143` is the smallest future lane, but the future comment must address closed-PR review history and ask for assignment. `#4124` is likely design/contract work. `#4192` may belong upstream of FastMCP.
- Duplicate-race: `#4246`, `#4161`, `#4205`, `#3839` and several adjacent lanes have active/closed PRs; do not race them.
- PR-readiness: no `PR-READY` target. Contribution process requires issue-first assignment before external PR.

## 3-Subagent Critique

- Factology/duplicates: `WATCH` is correct. `#4143/#4124` are open/unassigned, but both have closed unmerged process-gated PR history. Soften claims on `#4248`: it is an open covering PR, not recorded here as resolved/green.
- Process gates: `WATCH` satisfies AGENTS.md if the watch note records completed 6-subagent synthesis, parent live gates, 3-role critique, skipped runner reason, assignment-first blocker, and no upstream action.
- Actionability: `#4143` is the only lane close to a concrete comment-first ask, but only after current-main MRE and parent approval. If no upstream comment is posted now, final cycle status remains `WATCH`.

## Unblock Events

1. `#4143`: run current-main MRE in `corp-opensource-runner` or equivalent, verify stale stream-index behavior from `#4171`, draft a short issue comment proposing idempotent delete/test approach, and ask for assignment before any PR.
2. `#4124`: reproduce cancellation behavior on current main and ask maintainers for desired cancellation propagation contract before coding.
3. `#4192`: obtain Windows current-main repro and isolate whether FastMCP, MCP SDK, or `sse-starlette` owns the patch surface.
4. Re-run live duplicate/search gates immediately before any upstream comment/PR.
5. Do not open an upstream PR unless assignment or explicit maintainer approval is recorded.
