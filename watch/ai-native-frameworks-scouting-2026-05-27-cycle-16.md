# AI-native frameworks scouting: cycle 16 duplicate gate

Date: 2026-05-27
Mode: live follow-up on cycle 15 fallback candidates.

## Outcome

No new PR should be opened from this pass.

The best primary candidate remains `pydantic-ai#5688`, but it is still waiting on assignment/maintainer confirmation or the delegated pydanty PR opener.

## Live checks

| Candidate | Current state | Decision |
|---|---|---|
| `pydantic-ai#5688` | No assignment and no exact PR yet. Adjacent `#5664` is `httpx2` prep, not the `follow_redirects` closure bug. Our availability comment is latest human offer. | Watch. PR only after assignment/confirmation or if delegated opener stalls and maintainers invite external PR. |
| `google/adk-python#5864` | Still `request clarification`, assigned to maintainer. Our comment-offer remains unanswered. | Watch. No PR until maintainer/reporter direction. |
| `cline#11065` | Already covered by `cline#11084` from the issue author: `fix/plugin-sandbox): expose CLINE_PLUGIN_IMPORT_TIMEOUT_MS env override (#11065)`. | Duplicate-covered. No PR. |
| `modelcontextprotocol/python-sdk#2689` | Already covered by `python-sdk#2698`: `fix(client): respect negotiated capabilities in ClientSessionGroup`. | Duplicate-covered. No PR. |
| `opencode#29544` | Already covered by `opencode#29541`: `fix(opencode): emit SSE event ids`. | Duplicate-covered. No PR. |
| `CopilotKit#4935` | Covered by `CopilotKit#4947` and overlapping `#4948`: `sync streamed tool call args into messages`. | Duplicate-covered. No PR. |

## Active PR reminders

- `cline#11087`: still open and green; do not start another Cline PR unless the new target is both uncovered and clearly invited.
- `opencode#29565`: still open and green; avoid additional opencode PRs unless distinct and not duplicate-covered.
- `E2B#1354`: still blocked on CLA for `serejaris`.
- `OpenHands/software-agent-sdk#3394`: still waiting maintainer eval/workflow state.

## Decision

This pass converts several promising fallback ideas into `WATCH / duplicate-covered`.

Next safe work:

1. Monitor `pydantic-ai#5688` for a maintainer reply or pydanty PR.
2. If a pydanty PR appears, compare whether it tests the public `follow_redirects` factory contract before commenting.
3. If no PR appears and maintainers confirm external help, implement the narrow regression-first fix.

## Lesson

Fresh issue lists are not enough in fast AI repos. A candidate can become duplicate-covered between six-agent scoring and implementation. Always run a final PR search by issue number, symptom, branch name, and likely title immediately before coding.
