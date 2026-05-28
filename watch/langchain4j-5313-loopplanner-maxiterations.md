# langchain4j #5313 LoopPlanner maxIterations

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561093070)

Upstream issue: [langchain4j/langchain4j#5313](https://github.com/langchain4j/langchain4j/issues/5313)

Created: `2026-05-28`

Status: `CANDIDATE`

Upstream actions: `0`

Runner actions: `0`

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this cycle used the documented fallback: parent live GitHub/source gates, 6 read-only roles, then focused 3-role critique.

## Parent Gates

Live time: `2026-05-28 05:24-05:34 UTC`.

- Issue is open, unassigned, labeled `bug`, and had no comments at gate time.
- Repo `langchain4j/langchain4j` is Apache-2.0, active on `main`, and had about 12.1k stars / 2.3k forks at gate time.
- `CONTRIBUTING.md` exists. Bugfix PRs are allowed, but upstream asks contributors to open PRs as draft first, keep changes small, add tests, keep Java 17 compatibility, and run module/full checks.
- Exact open PR cover was not found for `#5313`, `LoopPlanner`, `maxIterations`, or `iterationsCounter`.
- Adjacent open PR `#5290` touches agentic scope serialization, not LoopPlanner iteration limits.

## Source Gate

Current `main` source at `langchain4j-agentic/src/main/java/dev/langchain4j/agentic/workflow/impl/LoopPlanner.java`:

- `iterationsCounter` starts at `1`.
- `nextAction()` checks `iterationsCounter > maxIterations` before it increments.
- With `maxIterations = N`, that allows the loop to enter the `(N + 1)`th round before `done()`.

This matches the reporter's minimal reproduction where `.maxIterations(3)` invokes the sub-agent/model 4 times.

## Runner Card

Runner is required before any upstream comment or PR. The dedicated `corp-opensource-runner` is still unavailable via [#10](https://github.com/serejaris/corp-opensource/issues/10); local environment has Node/Python only and is not the stable Java runner target for this repo.

Minimum runner evidence:

1. Fresh current `main`, JDK 17, no API keys.
2. Run targeted baseline: `./mvnw -pl langchain4j-agentic test`.
3. Add or execute a focused fail-before test proving `.maxIterations(3)` calls the sub-agent/model `4` times on current `main`, while the contract expects `3`.
4. Include one guard around multi-subagent or `testExitAtLoopEnd` semantics so the fix does not break loop behavior.
5. Repeat duplicate PR search for `#5313`, `LoopPlanner`, `maxIterations`, and `iterationsCounter`.

## 6-Role Synthesis

- Factology: `#5313` is the cleanest new uncovered lane in this slice. `langchain4j#5296` is also plausible, but its concurrency/race surface is heavier.
- Duplicate/race: no exact open cover was found for `#5313`. `mem0ai/mem0#5220` is not a candidate because open PRs `#5221` and `#5258` cover it directly.
- Process: internal tracking is allowed; upstream action is blocked until runner fail-before and repeat gates.
- Scope: this is less strategically central than Gemini/Vercel, but it is a small workflow/agent-runtime regression and likely cheaper than browser/CUA lanes once a Java runner is available.

## 3-Role Critique

- Factology/duplicates: approved `CANDIDATE`; source and issue evidence align, and no exact PR cover was found.
- Process gates: keep internal only. Upstream comment/PR is premature because no runner-backed fail-before or draft PR/test package exists, and the reporter offered to submit a PR.
- Actionability: do not create a dedicated tracker issue before runner evidence or maintainer signal. Keep it as a compact watch note and umbrella `#52` update.

## Decision

`next_status: CANDIDATE`

No upstream comment, PR, ping, rerun, or rebase.

No runner action in this cycle.
