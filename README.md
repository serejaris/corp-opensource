# Corp Open Source

Private control plane for open-source PR work: what we watch, who owns follow-up, what upstream decided, and which lessons should become reusable process.

## Active Watchlist

| Project | Upstream PR | Local/Fork PR | Status | Next action |
|---|---:|---:|---|---|
| NousResearch/hermes-agent | [#32884](https://github.com/NousResearch/hermes-agent/pull/32884) | [serejaris/hermes-agent:fix/codex-null-output-stream](https://github.com/serejaris/hermes-agent/tree/fix/codex-null-output-stream) | Closed as duplicate after fix landed via [#32963](https://github.com/NousResearch/hermes-agent/pull/32963) | Track whether upstream release includes the merged fix and whether openai-python follow-up lands |

## Current Rules

- Do not trust “we were first” as enough. Compare competing PRs for files, runtime paths, tests, and maintainer signals.
- Use subagents for fast duplicate triage when there are 2+ competing PRs:
  - Diff coverage: changed files, runtime paths, behavior.
  - Test coverage: regression tests, fixtures, validation commands.
  - Maintainer signal: labels, comments, reviews, timeline, linked issues, reactions.
- Post duplicate-check updates on the canonical PR when competing PRs appear.
- Reactions/upvotes are impact signal, not approval and not CI.
- Top-level PR comments can be actionable even when `reviews=[]`.

## Links

- Open-source PR workflow skill: `/Users/ris/.codex/skills/open-source-pr-workflow`
- Hermes PR we opened: https://github.com/NousResearch/hermes-agent/pull/32884
- Hermes PR merged by upstream: https://github.com/NousResearch/hermes-agent/pull/32963
- Competing broader PR: https://github.com/NousResearch/hermes-agent/pull/32890
- OpenAI Python SDK follow-up: https://github.com/openai/openai-python/pull/3315

## Issue Types

- `upstream-watch`: track a live external PR/issue.
- `duplicate-triage`: compare competing PRs and decide what to port.
- `skill-update`: capture process lessons that should update local skills.
- `release-followup`: check whether a merged upstream fix reached a release.

