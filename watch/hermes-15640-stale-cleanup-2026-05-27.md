# Hermes #15640 stale cleanup, 2026-05-27

Tracker: [#67](https://github.com/serejaris/corp-opensource/issues/67). Upstream: [`NousResearch/hermes-agent#15640`](https://github.com/NousResearch/hermes-agent/pull/15640), issue [`#15636`](https://github.com/NousResearch/hermes-agent/issues/15636).

Final `next_status`: `PR-OPEN / WATCH-stale-clean`.

Upstream action in this cycle: 0 PR/comment/rerun/rebase/close.

## Parent live gates

- Gate time: `2026-05-27 21:15 UTC`.
- Repo: active, not archived/fork, default branch `main`, MIT, `170139` stars, `28435` forks, `4915` issues, `9474` PRs.
- Repo activity: pushed `2026-05-27 20:55 UTC`; latest release `v2026.5.16`, published `2026-05-16`.
- PR `#15640`: open, non-draft, `MERGEABLE`, `mergeStateStatus: CLEAN`; no comments, reviews, assignee, or check rollup; labels `type/bug`, `comp/cli`, `comp/gateway`, `P2`; last updated `2026-04-25`.
- Issue `#15636`: open, same labels, no comments or assignee; last updated `2026-04-25`.
- Scope: macOS launchd plist should use the venv `hermes` console script as `ProgramArguments[0]` so macOS Login Items shows `hermes`, not `python`.

## Duplicate / drift gate

Exact duplicate not found for the Login Item `python`/`hermes` attribution bug. Do not mark this as duplicate-covered.

Nearby open gateway/launchd queue is crowded and raises drift/merge-risk:

- `#29673` names macOS launchd gateways by profile.
- `#33020` avoids hanging launchd restarts.
- `#28530` extends launchd exit timeout.
- `#27691` verifies launchd restart relaunches.
- `#20489` refuses launchd bootout from gateway child.
- `#27860` preserves proxy env in launchd plist.
- `#26971` detects launchd service via print fallback.
- Other gateway/launchd noise exists in the same queue; re-run search before any close/ping.

## 3-subagent critique

- Factology/duplicates: `MERGEABLE/CLEAN` is only technical state, not maintainer acceptance. Exact duplicate was not found; nearby gateway/launchd work is drift/collision risk, not proof of coverage.
- Process/runner: no upstream ping, close, rebase, rerun, or force-push is justified from this cleanup check. Linux runner would not prove the macOS Login Item UX; fresh macOS launchd revalidation is required if we choose to revive/ping.
- Actionability: move from generic stale backlog to internal `CLOSE-CANDIDATE`. Do not close upstream now; a separate cleanup cycle must decide close vs one useful ping vs keep watch.

## Decision

Internal cleanup status is `CLOSE-CANDIDATE` because the PR is ours, open/clean, but fully stale since `2026-04-25`, has no maintainer signal/checks/reviews/comments, and is outside the current Hermes/Codex outage flow.

The PR should not stay in active monitor rows as if normal maintainer review is expected. It should live in stale cleanup backlog until one of these happens:

1. Maintainer comments/reviews `#15640` or `#15636`.
2. A fresh macOS launchd revalidation proves the fix is still current and worth a single maintainer ping.
3. A cleanup cycle chooses to close the PR with a neutral stale/out-of-current-scope comment.
4. A nearby launchd/gateway PR lands and makes the fix obsolete or requires a rebase/rewrite.

## Follow-up 23:51 UTC

Bounded cleanup refresh for `NousResearch/hermes-agent#15640`: 6 read-only subagents, parent live gates via `gh`, then 3-role critique. No upstream PR/comment/ping/close/rebase/rerun was made.

Tracker update: [#67 comment](https://github.com/serejaris/corp-opensource/issues/67#issuecomment-4559657466).

Live gates:

- PR `#15640`: still open, non-draft, `MERGEABLE/CLEAN`; labels `type/bug`, `comp/cli`, `comp/gateway`, `P2`; no assignee, comments, reviews, or check rollup; last updated `2026-04-25T13:13:08Z`.
- Issue `#15636`: still open, same labels, no assignee/comments; last updated `2026-04-25T13:13:00Z`.
- Exact duplicate PR `#15885`: closed unmerged on `2026-04-26T05:07:54Z`; collaborator called it duplicate of `#15640`, and the author closed it as superseded by `#15640`, explicitly noting `#15640` has fallback safety, broader test coverage, and stronger rationale.
- Collision/drift risks: `#29673` is open and related to `#15636/#15640`, solving display-name class via a profile-scoped wrapper; `#29965` is open and broader macOS app-identity work. Other launchd/gateway PRs such as `#33020`, `#28530`, `#27691`, `#20489`, `#27860`, `#26971`, and `#6435` are adjacent drift/merge-risk, not exact coverage.

6-subagent synthesis:

- Factology: `#15640` remains stale but technically clean and open; `#15885` strengthens, rather than weakens, `#15640` as the preferred exact lane.
- Duplicate/drift: not duplicate-covered. Do not close as covered. Collision risk is high due to newer launchd/app-identity work.
- Process: do not ping without a new fact; do not close without an explicit stale/out-of-current-scope decision; do not rebase/rerun while clean and unrequested.
- Runner/repro: fresh macOS launchd validation is required before claiming the fix is still-current or asking for review/merge. CT `216` cannot prove macOS Login Items / Background Items behavior.
- Actionability: internal-only update now; upstream action count `0`.

3-role critique:

- Factology/duplicates: approved. `#15885` is closed as superseded by `#15640`, while `#29673/#29965` are collision risks, not exact duplicate coverage.
- Process gates: approved for tracker/watch update only; no upstream close/ping/rebase/rerun without maintainer signal or fresh macOS validation.
- Actionability: revise from `CLOSE-CANDIDATE` to `PR-OPEN / WATCH-stale-clean`. The PR is stale, but still open/clean and not disproven.

Updated decision: `next_status: PR-OPEN / WATCH-stale-clean`.

Triggers:

1. Maintainer comment/review on `#15640` or `#15636`.
2. `#29673` or `#29965` merges/closes and changes the launchd identity surface.
3. Fresh macOS launchd validation proves `#15640` still-current; then one short maintainer ping may be justified.
4. PR becomes conflicted/failing.
5. A later cleanup decision explicitly closes the PR as stale/out-of-current-scope without claiming the fix is disproven.
