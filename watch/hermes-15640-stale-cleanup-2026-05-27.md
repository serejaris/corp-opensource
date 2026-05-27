# Hermes #15640 stale cleanup, 2026-05-27

Tracker: [#67](https://github.com/serejaris/corp-opensource/issues/67). Upstream: [`NousResearch/hermes-agent#15640`](https://github.com/NousResearch/hermes-agent/pull/15640), issue [`#15636`](https://github.com/NousResearch/hermes-agent/issues/15636).

Final `next_status`: `CLOSE-CANDIDATE`.

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
