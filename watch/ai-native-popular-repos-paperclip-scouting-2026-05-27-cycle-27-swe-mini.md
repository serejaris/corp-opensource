# Popular AI-native / Paperclip-like scouting cycle 27: SWE-agent mini, 2026-05-27

## Summary

- Target: `SWE-agent/mini-swe-agent` with cross-check against `SWE-agent/SWE-agent`.
- Tracker: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52) ([cycle comment](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4557324872)), candidate lane [#53](https://github.com/serejaris/corp-opensource/issues/53).
- Parent live gates: 2026-05-27 18:05 UTC via `gh repo view`, `gh issue view`, `gh pr list`, direct issue/PR list checks.
- Upstream actions: 0 PR, 0 comments.
- Final `next_status`: `WATCH`.

`open-source-bug-scouting` and `open-source-pr-workflow` are not installed in this Codex environment, so this cycle used the repo-required fallback: 6 scouting subagents, parent live gates, 3-subagent critique, and local tracker/watch update only.

## Parent Live Gates

| Gate | Result |
|---|---|
| `SWE-agent/mini-swe-agent` repo state | 4612 stars, latest release `v2.3.0`, pushed 2026-05-25, 17 open issues, 9 open PRs, default branch `main`. |
| `SWE-agent/SWE-agent` cross-check | 19340 stars, latest release `v1.1.0`, pushed 2026-05-25, 28 open issues, 26 open PRs. Use only as duplicate/design cross-check in this cycle. |
| Primary issue | [`mini-swe-agent#826`](https://github.com/SWE-agent/mini-swe-agent/issues/826) open bug, no assignee/comments at live gate: agent-spawned scripts keep running after bash command timeout. |
| Secondary issue | [`mini-swe-agent#829`](https://github.com/SWE-agent/mini-swe-agent/issues/829) open bug around Dash launched as `/bin/sh`; maintainer-facing direction likely needed before a narrow PR. |
| FormatError cluster | `#727/#737/#784` overlap with open PRs [`#728`](https://github.com/SWE-agent/mini-swe-agent/pull/728) and [`#821`](https://github.com/SWE-agent/mini-swe-agent/pull/821); no new PR lane. |
| Duplicate search caveat | Direct issue/PR pass found no exact competing PR for `#826`, but GitHub Search API returned rate limit 403, so broader duplicate search is incomplete. |
| Runner gate | Not satisfied. Subagent temp-clone repro is useful signal, but `corp-opensource-runner` repro is still required before any upstream status change. |

## 6-Subagent Synthesis

| Role | Finding |
|---|---|
| repo-fit | `mini-swe-agent` is a strong Paperclip-adjacent lane: compact terminal-agent runtime, active maintainer, pytest/ruff/pre-commit surface, and AI-agent process/tool-call issues. Broader `SWE-agent/SWE-agent` has more backlog and PR noise; use as cross-check. |
| bug-signal | Best issue-level signal is `#826`: clear runtime symptom, no public assignee/comment, and likely process-tree timeout root cause. `#829` is plausible but maintainer wording suggests comment-first. FormatError issues are duplicate/design-risk. |
| repro-path | Secret-free temp-clone repro observed a timed-out command leaving `python3 /tmp/mini_swe_agent_826_leak.py` with `PPID=1`. Source path: `src/minisweagent/environments/local.py`, `LocalEnvironment.execute()` uses `subprocess.run(command, shell=True, timeout=...)`. |
| patchability | Minimal patch likely needs process-group/session handling around local shell execution and a regression for child cleanup after timeout. POSIX/Windows semantics must be handled explicitly. |
| duplicate-race | No direct competing PR found in direct lists for `#826`; adjacent timeout issues `#803/#807/#832` appear distinct. This remains caveated because GitHub search was rate-limited. |
| PR-readiness | Not PR-ready. Contribution/test surface is clear, but regression card, runner repro, and fresh duplicate gate are missing. |

## Candidate Ranking

| Candidate | Status | Why |
|---|---|---|
| [`mini-swe-agent#826`](https://github.com/SWE-agent/mini-swe-agent/issues/826) | `WATCH`, promoted lane | Strongest current candidate, but blocked by dedicated runner repro and repeated duplicate search after rate-limit. |
| [`mini-swe-agent#829`](https://github.com/SWE-agent/mini-swe-agent/issues/829) | `COMMENT-FIRST` later | Repro signal exists on Dash-based `/bin/sh`, but maintainer direction is needed on whether to change shell invocation or documentation/prompt contract. |
| FormatError cluster `#727/#737/#784/#821/#728` | `WATCH` / no new PR | Active open PRs already cover important parts; design and duplicate risk too high. |
| `SWE-agent/SWE-agent` parser/tool PR queue | `WATCH` only | Many active PRs around function-call parsing and tool installation; avoid competing PRs in this cycle. |

## 3-Subagent Critique

| Role | Result |
|---|---|
| factology / duplicates | Do not claim duplicate-clean. Correct wording: direct issue/PR pass found no exact competing PR, but GitHub search was rate-limited. `#803/#807/#832` are adjacent, not exact duplicates based on current evidence. |
| process gates | Fails for upstream action, passes for tracker/watch update. Missing gates: parent-level runner repro, complete GitHub search, and regression card. |
| actionability | Choose exactly `WATCH` now. Unblock event: runner repro for `#826`, repeated search after rate-limit, and regression card with pre-fix failure. |

## Next Bounded Cycle

1. Run secret-free `#826` repro in `corp-opensource-runner`.
2. Repeat GitHub issue/PR search once API rate-limit clears.
3. Fill regression card: contract, test file, command, pre-fix fail, post-fix pass expectation.
4. Run 3-subagent critique again before any upstream comment or PR.
