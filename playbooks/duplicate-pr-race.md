# Duplicate PR Race Playbook

Use this when several PRs target the same upstream outage.

## Subagents

Run these in parallel:

1. Diff coverage: compare files, runtime paths, helper functions, and behavior.
2. Test coverage: compare regression tests, fixtures, CI, and validation claims.
3. Maintainer signal: read labels, comments, reviews, timeline, linked issues, reactions, and maintainer wording.

## Decision

After synthesis, do exactly one:

- Port missing behavior/tests into the canonical PR.
- Ask maintainers which PR should consolidate.
- Post a duplicate-check update saying no extra behavior needs to be pulled in.

## Duplicate-check comment shape

```text
Duplicate check update: I compared the newer duplicate PRs #A and #B.

- #A covers the same parser guard and adds X regression.
- #B covers the same minimal guard.

#CURRENT already includes the parser guard/regression and additionally covers Y, so I did not find extra behavior from those PRs that needs to be pulled into this branch.
```

