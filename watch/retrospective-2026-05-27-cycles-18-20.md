# Retrospective: scouting cycles 18-20

Date: 2026-05-27
Scope: three recent subagent cycles for AI-native open-source contribution scouting.

## Summary

The process is now useful, but still too optimistic unless parent duplicate checks override subagent suggestions.

The best output from these cycles was not just a new PR. It was a sharper operating loop:

1. subagents generate candidate breadth;
2. parent performs live duplicate/timeline gate;
3. only then regression card / upstream comment / PR work starts;
4. README dashboard tracks what actually happened.

## Cycle 18

Outcome:

- Six-role research found `goose#9446` as the cleanest actionable target.
- Parent prepared regression-first patch and opened `aaif-goose/goose#9447`.
- `e2b-dev/E2B#1354` was closed as superseded by maintainer/superset `#1355`.

What worked:

- Good target selection: `goose#9446` was fresh, Codex/Hermes-adjacent, unassigned, and fixture-testable.
- Regression-first worked: pre-fix failure was captured (`missing field output`) and post-fix targeted tests passed.
- Duplicate cleanup worked: E2B was closed rather than defended.

What failed / risk:

- CI red state required interpretation. The `changes` job failed on GitHub diff API, not code. This must be tracked explicitly to avoid fake code-fix work.

## Cycle 19

Outcome:

- No new PR opened.
- Several tempting candidates were rejected after live parent duplicate checks:
  - MCP TS `#1582` -> covered by open `#1657`;
  - trycua `#1722` -> covered by author PR `#1723`;
  - MCP Python `#2687` -> covered by open `#2701`;
  - OpenHands SDK `#3317` -> previous exact PR `#3319`, and our `#3394` still active in same repo.

What worked:

- Parent duplicate gate caught subagent false positives.
- The loop prevented noisy duplicate PRs.
- README/watch notes were updated with exact status rather than vague “candidate”.

What failed / risk:

- One subagent proposed MCP Python `#2687` as clean even though `#2701` existed. Lesson: subagent search queries are not authoritative unless timeline/linked PRs are checked by parent.
- “No search results” is not enough; GitHub text search missed linked PRs that issue timeline revealed.

## Cycle 20 / Continue pass

Outcome:

- Subagents surfaced new candidates:
  - Continue `#12334`;
  - Agno `#7871/#8113`;
  - ADK `#5869`;
  - Cline `#10927`;
  - others.
- Parent selected Continue `#12334` as the cleanest actionable next candidate:
  - no assignee;
  - no linked PR;
  - no duplicate PR from search;
  - no secrets needed;
  - contribution guide requires issue/comment before code.
- Upstream plan comment posted.
- Internal tracker `corp-opensource#50` and watch note created.
- Local clone failed/stopped due GitHub HTTP 502, not a code blocker.

What worked:

- The process respected repo rules: comment before code.
- The README dashboard now starts with active PRs and closed/not-merged outcomes, so progress is visible.
- Failed clone state was cleaned up and recorded honestly.

What failed / risk:

- Agno `#7871` looked like a perfect tiny fix from patchability pass, but timeline showed assignment and multiple linked PRs. Patchability alone is not enough.
- Clone/network failure interrupted momentum. For large repos, use shallow/filter clone first:

```bash
git clone --filter=blob:none --depth 1 https://github.com/serejaris/continue.git /Users/ris/Documents/GitHub/continue
```

If that still fails, retry later rather than burning time.

## Updated operating rules

1. Never treat a subagent `GO` as actionable until parent checks:
   - issue assignees;
   - issue comments;
   - issue timeline cross-references;
   - `gh search prs`;
   - current open PR list for adjacent terms.
2. Search result `[]` is weak evidence. Timeline evidence is stronger.
3. If a repo requires “comment before code”, post a concise regression plan before cloning or patching.
4. README must start with active PR dashboard:
   - active upstream PRs;
   - closed/not-merged PRs;
   - current in-progress candidate.
5. Do not open a second PR in a repo where our earlier PR is waiting for maintainer/eval unless the new issue is clearly independent and urgent.
6. For every candidate, create or update a watch note before implementation:
   - contract;
   - duplicate gate;
   - contribution gate;
   - regression card;
   - local checkout state.

## Next best action

Continue with `continuedev/continue#12334` when resuming:

1. retry shallow/filter clone;
2. locate autocomplete request path and test surface;
3. write failing regression that captures outbound OpenRouter autocomplete body;
4. patch only if pre-fix failure is proven;
5. run targeted tests and typecheck/format commands;
6. open ready PR if checks pass.

