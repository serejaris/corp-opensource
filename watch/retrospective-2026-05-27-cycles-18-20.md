# Retrospective: scouting cycles 18-19 plus Continue pass

Date: 2026-05-27
Scope: two documented six-subagent cycles (`cycle 18`, `cycle 19`) plus the following Continue `#12334` candidate pass. The filename keeps the original `cycles-18-20` shorthand, but there is no separate `cycle-20` watch note.

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
- README now shows exact status rather than vague “candidate”.

What failed / risk:

- One subagent proposed MCP Python `#2687` as clean even though `#2701` existed. Lesson: subagent search queries are not authoritative unless timeline/linked PRs are checked by parent.
- “No search results” is not enough; GitHub text search missed linked PRs that issue timeline revealed.

## Continue pass

Outcome:

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

- Several non-Continue leads arrived from subagents in chat notifications, but they were not captured in a dedicated `cycle-20` watch note. Do not treat those issue numbers as verified in this retrospective until they get their own duplicate/timeline gate.
- Clone/network failure interrupted momentum. For large repos, use shallow/filter clone first:

```bash
git clone --filter=blob:none --depth 1 https://github.com/serejaris/continue.git /Users/ris/Documents/GitHub/continue
```

If that still fails, retry later rather than burning time.

## 3-subagent critique cycle

After the first retrospective draft, a separate 3-subagent critique pass reviewed it:

1. **Fact-check critique** found that the draft overstated “cycle 20” as if a dedicated watch note existed. It also flagged unsupported claims about Agno/ADK/Cline candidates that were present in chat notifications but not in the durable notes.
2. **Process critique** found that subagent `GO` remained too trusted. It recommended the rule: subagents produce `LEAD / WATCH / NO-GO`; only the parent can promote to `CANDIDATE` after live gates.
3. **Actionability critique** found the Continue handoff too vague: it needed owner, evidence links, exact clone command, blocker status, search commands, and acceptance criteria.

Corrections made from critique:

- Retrospective scope changed to “cycles 18-19 plus Continue pass”.
- Unsupported candidate list removed from the Continue pass.
- Continue handoff made explicit below.
- README scouting role list updated to all six roles.

## Continue handoff

Owner: next `corp-opensource` parent agent.

Current candidate: `continuedev/continue#12334`.

Last verified: 2026-05-27 12:50 UTC.

Evidence:

- Upstream issue: https://github.com/continuedev/continue/issues/12334
- Upstream plan comment: https://github.com/continuedev/continue/issues/12334#issuecomment-4554636607
- Internal tracker: https://github.com/serejaris/corp-opensource/issues/50
- Watch note: [continue-12334-openrouter-autocomplete-extra-body.md](continue-12334-openrouter-autocomplete-extra-body.md)

Known blocker:

- First clone attempt failed with GitHub HTTP 502 / `expected packfile`.
- Shallow retry was stopped cleanly when the user paused the run.
- No partial checkout remains.

Resume command:

```bash
git clone --filter=blob:none --depth 1 https://github.com/serejaris/continue.git /Users/ris/Documents/GitHub/continue
```

If clone fails twice with GitHub/network errors, stop as `network-blocked` and update `corp-opensource#50`; do not burn time retrying.

Initial code search after clone:

```bash
rg "extraBodyProperties|OpenRouter|autocomplete|completion" core packages extensions -S
rg "requestOptions" core packages extensions -S
```

Acceptance criteria before PR:

- A failing regression proves the OpenRouter autocomplete outbound request body is missing `requestOptions.extraBodyProperties.reasoning`.
- The patch changes only request construction/propagation for the affected path.
- Post-fix test proves outbound body includes the configured `reasoning` object:

```json
{
  "reasoning": {
    "exclude": true,
    "enabled": false,
    "effort": "none"
  }
}
```

- Exact verification commands are still unknown until the repo is cloned and test surface is located. Do not claim `npm --prefix core test` coverage until the relevant test file is identified and run.

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
7. Subagent outputs must end in one of: `LEAD`, `WATCH`, `NO-GO`. Only the parent promotes a lead to `CANDIDATE`.
8. Patchability is capped at `WATCH` when there is an assignee, linked PR, maintainer comment saying someone is working, or an active same-repo PR by us waiting for review.
9. Record stop labels explicitly: `upstream-infra-red`, `network-blocked`, `duplicate-blocked`, `maintainer-motion-blocked`, `waiting-upstream`, `ready-next`.

## Next best action

Continue with `continuedev/continue#12334` when resuming:

1. retry the shallow/filter clone command from the handoff block;
2. locate autocomplete request path using the search commands above;
3. identify the exact targeted test command before patching;
4. write failing regression that captures outbound OpenRouter autocomplete body;
5. patch only if pre-fix failure is proven;
6. run targeted tests and relevant typecheck/format commands;
7. open ready PR if checks pass.
