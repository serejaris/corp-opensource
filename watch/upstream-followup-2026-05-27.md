# Upstream Follow-up 2026-05-27

## Check 10:35 -03

Live follow-up after opening `opencode#29565`.

| Upstream | Current state | Action |
|---|---|---|
| `anomalyco/opencode#29565` | Open, ready, mergeable. `check-standards`, `check-compliance`, `check-duplicates`, `add-contributor-label` all green. No comments/reviews yet. | Watch maintainer review; do not open another opencode PR unless a distinct gap appears. |
| `anomalyco/opencode#29530` | Open, mergeable. Duplicate/compliance/standards checks green; GitHub bot says contribution guidelines pass. | Watch maintainer review. |
| `OpenHands/software-agent-sdk#3394` | Open. Maintainer said it looks fine but wants eval. Latest fork workflows on `3c8eae78` are still `action_required`, no jobs. | Wait maintainer eval/workflow approval. No code action. |
| `e2b-dev/E2B#1354` | Open, mergeable, review required. CLA check still `ERROR`; Vercel auth still team-side. | User must sign E2B CLA, then comment `@cla-bot check`. |
| `cline/cline#11087` | Open, mergeable. Greptile gap addressed; visible SDK/quality/security checks green or skipped intentionally. | Watch maintainer review. |
| `CopilotKit/CopilotKit#5035` | Open, mergeable, review required. Docs Vercel preview green; other Vercel previews need team authorization; Claude automated review disabled for fork. | Watch maintainer review / team preview authorization. |
| `pydantic/pydantic-ai#5678` | Open, mergeable. CI matrix, coverage, smokeshow green. Codex review quota warning only. | Watch maintainer review. |
| `pydantic/pydantic-ai#5680` | Open, mergeable. CI matrix, coverage, smokeshow green. Codex review quota warning only. | Watch maintainer review. |
| `pydantic/pydantic-ai#5681` | Open, mergeable. CI matrix, coverage, smokeshow green. | Watch/review only; duplicate-covered lane. |
| `langchain-ai/deepagents#3616` | Closed by assignment guard. Checks cancelled/failing because PR was auto-closed, not code signal. | No second PR until maintainer assignment/reopen. |

## Decision

No active upstream PR needs a code fix right now. Continue strategy by running the next scouting cycle while monitoring existing PRs.

## Check 08:10 -03

Live follow-up after cycle 15 scouting.

| Upstream | Current state | Action |
|---|---|---|
| `pydantic-ai#5688` | Open; pydanty confirmed root cause and says `PR-ready -> delegating to PR opener`; no exact open PR found. | Comment-offer posted; wait assignment/confirmation or pydanty PR before opening anything. |
| `google/adk-python#5864` | Open; our comment-offer is the latest external action; still `request clarification`. | Wait maintainer/reporter direction. |
| `OpenHands/software-agent-sdk#3394` | Open/mergeable; no status checks reported; maintainer wants eval; old bot `CHANGES_REQUESTED` review remains in GitHub reviewDecision. | Wait eval / maintainer action. |
| `E2B#1354` | Still CLA-blocked for `serejaris`; Vercel auth is team-side. | User must sign CLA, then comment `@cla-bot check`. |

Decision: no new PR now. Best next technical branch is `pydantic-ai#5688`, but only after assignment/confirmation because bot delegation is already in flight.
