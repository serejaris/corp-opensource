# Upstream Follow-up 2026-05-27

## Check 09:05 -03

Live follow-up after cycle 18 scouting and E2B duplicate cleanup.

| Upstream | Current state | Action |
|---|---|---|
| `e2b-dev/E2B#1354` | Closed by us as superseded. It only covered JS `Template.fromDockerfile` and was CLA-blocked. | Done. Track `#1355` instead. |
| `e2b-dev/E2B#1355` | Open maintainer/superset PR. Covers JS `Template.fromDockerfile` and Python `Template.from_dockerfile`, includes multi-source `COPY`/`ADD` regressions, CLA and Vercel green; SDK checks still in progress in tracked state. | Watch `#1355`; no duplicate PR. |
| `modelcontextprotocol/python-sdk#2701` | Open, mergeable. Covers `#2687` with redirect URI normalization and shared auth regression; most CI green, one Windows locked job failed while all-green reported success. | Watch existing PR; no new PR for `#2687`. |
| `pydantic-ai#5678/#5680/#5681` | Open, mergeable, CI green in tracked state. | Watch. |
| `pydantic-ai#5688` | Still open, no assignee, pydanty root-cause comment + our offer; no exact PR found in tracked state. | Wait assignment/confirmation; local patch prep exists. |
| `google/adk-python#5864` | Still open, assigned, `request clarification`; no reply after our offer. | Wait maintainer/reporter direction; local patch prep exists. |
| `langgraph#7688` | Still open; our TIME_WAIT repro comment now has one thumbs-up; no maintainer direction yet. | Watch for package-surface direction. |
| `OpenHands/software-agent-sdk#3394` | Open, mergeable, maintainer wants eval; reviewDecision still `CHANGES_REQUESTED` due earlier bot review despite follow-up. | Wait eval/human maintainer. |
| `opencode#29530/#29565`, `cline#11087`, `CopilotKit#5035` | Still open; no new code-action comments in tracked state. | Watch maintainer review. |

## Check 09:20 -03

Live follow-up after opening `goose#9447`.

| Upstream | Current state | Action |
|---|---|---|
| `aaif-goose/goose#9447` | Open, ready, mergeable, review required. `check-quarantined` and `machete` passed. `changes` failed because GitHub API returned `diff temporarily unavailable due to heavy server load`; dependent Rust/release/live-provider jobs skipped. Rerun attempt via `gh run rerun --failed` was not allowed. Local `cargo clippy -p goose --lib -- -D warnings` passed; PR comment posted with CI note. | Watch CI/review; ask maintainer rerun if needed. |
| `aaif-goose/goose#9446` | PR `#9447` linked as fix for the missing `response.output` parser failure. | Track through PR. |

Decision: no fresh upstream PR this pass. The productive move was closing the now-duplicate E2B PR and recording new candidates for the next regression-first pass.

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

## Check 08:25 -03

Live duplicate gate on the fallback candidates from cycle 15.

| Candidate | Current state | Action |
|---|---|---|
| `cline#11065` | Covered by `cline#11084` from the issue author. | No PR. |
| `modelcontextprotocol/python-sdk#2689` | Covered by `python-sdk#2698`. | No PR. |
| `opencode#29544` | Covered by `opencode#29541`. | No PR. |
| `CopilotKit#4935` | Covered by `CopilotKit#4947` and overlapping `#4948`. | No PR. |
| `pydantic-ai#5688` | Still open, no assignment, no exact PR found; our availability comment remains latest. | Watch; do not race pydanty delegation. |
| `google/adk-python#5864` | Still `request clarification`; no reply to our PR offer. | Watch. |

Decision: keep the queue in watch mode. The next implementation opportunity is still `pydantic-ai#5688`, but only after repo gate opens or after comparing the delegated PR if it appears.
