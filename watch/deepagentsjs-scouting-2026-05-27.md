# langchain-ai/deepagentsjs scouting, 2026-05-27

Live state: parent `gh` pass на 2026-05-27 20:05-20:12 UTC.

Upstream: [`langchain-ai/deepagentsjs`](https://github.com/langchain-ai/deepagentsjs).
Internal tracker: [`corp-opensource#62`](https://github.com/serejaris/corp-opensource/issues/62).
Umbrella tracker: [`corp-opensource#52`](https://github.com/serejaris/corp-opensource/issues/52).

Upstream actions в этом блоке: 0 PR, 0 comments.

Final `next_status`: `WATCH`.

Required skills `open-source-bug-scouting` и `open-source-pr-workflow` в текущей session недоступны, поэтому использован documented fallback: 6 read-only scouting subagents, parent live gates, затем 3-subagent critique.

## Parent Live Gates

| Gate | Result |
|---|---|
| Repo state | `langchain-ai/deepagentsjs`: 1270 stars, 202 forks, default branch `main`, not archived, not fork, latest release [`deepagents@1.10.2`](https://github.com/langchain-ai/deepagentsjs/releases/tag/deepagents%401.10.2) published `2026-05-13T13:39:33Z`, pushed `2026-05-22T23:37:27Z`, updated `2026-05-27T15:03:13Z`. |
| Queue pressure | About 30 open issues and 33 open PRs. Fresh high-impact PR queue includes [`#555`](https://github.com/langchain-ai/deepagentsjs/pull/555), [`#554`](https://github.com/langchain-ai/deepagentsjs/pull/554), [`#551`](https://github.com/langchain-ai/deepagentsjs/pull/551), [`#541`](https://github.com/langchain-ai/deepagentsjs/pull/541), plus older open `#288/#463/#247`. |
| Product fit | High Paperclip-like / AI-native fit: JS agent harness with subagents, skills, QuickJS/REPL, sandbox/filesystem backends, streaming, browser/runtime compatibility, and LangGraph integration. |
| Process gate | Inherited LangChain `CONTRIBUTING` says external PRs must link to an issue/discussion where a maintainer approved the solution. Cold PRs can be closed. No local PR template was found, so any future body must manually include approval link, problem, fix, tests, risk, and linked issue. |
| CI / test gate | CI uses `pnpm install --frozen-lockfile`, `pnpm format:check`, `pnpm run lint`, `pnpm build`, and `pnpm test`. Unit CI runs Ubuntu/Windows with Node 22.x and 24.x. Integration tests require secrets and are skipped for fork PRs. |
| Security gate | Org-level security policy exists; security-impact reports should go through responsible disclosure, not public PR-first. |
| Strongest uncovered lead | [`#547`](https://github.com/langchain-ai/deepagentsjs/issues/547): `new StateBackend()` crashes on `write_file` / `edit_file`, while `(runtime) => new StateBackend(runtime)` works. Open, no assignee/labels/comments, no direct competing PR found in this pass. |
| Covered fresh lanes | `#553` is covered by open [`#554`](https://github.com/langchain-ai/deepagentsjs/pull/554); `#550` by open [`#551`](https://github.com/langchain-ai/deepagentsjs/pull/551); `#542` by open [`#541`](https://github.com/langchain-ai/deepagentsjs/pull/541). |
| Covered older lanes | `#277` is covered by open unmerged [`#288`](https://github.com/langchain-ai/deepagentsjs/pull/288); `#387` by open [`#463`](https://github.com/langchain-ai/deepagentsjs/pull/463); `#241` by open conflicting [`#247`](https://github.com/langchain-ai/deepagentsjs/pull/247). |

## 6-Subagent Synthesis

| Role | Finding |
|---|---|
| repo-fit | High-fit watch: active LangChain JS harness repo with subagents, skills, browser/runtime, sandbox and provider edge cases. Risks: fast-moving surface, uneven review latency, and high duplicate-race. |
| issue triage | `#547` is the cleanest uncovered lead. `#277` looked narrow but is already covered by open `#288`. `#397/#259/#544` are possible future lanes but need latest repro or maintainer preference. |
| PR / duplicate pressure | Fresh obvious fixes already have PRs. Skills/QuickJS cluster `#540/#535/#549/#463/#554/#546` is crowded. Middleware/provider cluster `#555/#551/#493/#526` overlaps. |
| process gates | Approval-first rule blocks cold PR. Future action must be comment-first or maintainer-approved PR after runner/current-main evidence. |
| repro / runner | Secret-free unit tests are available with pnpm/Vitest, but shell/sandbox/backend integration belongs in runner with sanitized env. Provider/cloud sandbox integrations need secrets and should not be default evidence. |
| PR-readiness | No `PR-READY`. One scout raised `#547` to internal candidate; actionability and process critiques kept final repo cycle at `WATCH` until repro and approval gate. |

## 3-Subagent Critique

| Role | Result |
|---|---|
| factology / duplicates | `#547` has no direct competing PR found, but uniqueness is search-based, not proven root cause. `#277` must be described as covered by open unmerged `#288`, not resolved. `#553/#550/#542` coverage by `#554/#551/#541` is correct. |
| process gates | Final should not be `CANDIDATE` or `COMMENT-FIRST` unless one issue has runner repro, regression card, duplicate gate, and maintainer-approved solution path. Approval-first contribution rule is a hard blocker for PR. |
| actionability | Upstream comment on `#547` now would add no new fact beyond an already strong issue. Keep `#547` as internal lead; no upstream action until current-main repro and narrowed maintainer question. |

## Decision

Keep `langchain-ai/deepagentsjs` as high-fit active watch. `#547` is the strongest next issue-level lane, but this cycle remains repo-level.

Final `next_status`: `WATCH`.

Promotion gates for `#547`:

1. Run current-main repro in `corp-opensource-runner` with sanitized env and targeted `deepagents` unit/integration tests.
2. Recheck exact duplicate/PR search for `StateBackend`, `write_file`, `edit_file`, and factory/runtime backend path.
3. Decide if the correct upstream ask is code fix or docs/API clarification.
4. If evidence adds a new fact, move to `COMMENT-FIRST` with a narrow maintainer question; do not PR without maintainer approval.
