# AI-native Frameworks Scouting Cycle 22

Checked at `2026-05-27T14:10Z`.

## Method

Bounded follow-up cycle по текущей очереди:

1. 6-subagent scouting/follow-up:
   - repo-fit;
   - bug-signal;
   - repro-path;
   - patchability;
   - duplicate-race;
   - PR-readiness.
2. Parent live gates:
   - issue state, assignee/labels;
   - linked/timeline PRs и direct PR view;
   - PR search;
   - active same-repo PR pressure;
   - contribution/process gate;
   - regression card status.
3. 3-subagent critique:
   - fact-check;
   - process-gate;
   - actionability.

No external upstream PR/comment was opened in this cycle.

## Synthesis

| Target | Live findings | Decision | Next action |
|---|---|---|---|
| [modelcontextprotocol/typescript-sdk#2115](https://github.com/modelcontextprotocol/typescript-sdk/issues/2115) | Parent gate corrected cycle 21: exact competing PRs [#2116](https://github.com/modelcontextprotocol/typescript-sdk/pull/2116) and [#2138](https://github.com/modelcontextprotocol/typescript-sdk/pull/2138) are both open, mergeable, review required, checks green. Both touch `packages/core/src/shared/protocol.ts`, add regression coverage in `packages/core/test/shared/protocol.test.ts`, and add a changeset for `@modelcontextprotocol/core`. | `WATCH / duplicate-race` | No PR. Watch maintainer choice/merge/close between `#2116` and `#2138`; re-run duplicate gate before any new upstream action. |
| [continuedev/continue#12334](https://github.com/continuedev/continue/issues/12334) | Issue still open; no reporter clarification after our evidence comment. Exact PR for OpenRouter autocomplete `extraBodyProperties` not found. Adjacent PRs do not prove the reported autocomplete body path. | `WATCH / needs-reporter-path` | Wait for Continue version/build and exact outbound autocomplete path/body. |
| [aaif-goose/goose#9447](https://github.com/aaif-goose/goose/pull/9447) | PR open, mergeable, review required. `check-quarantined` and `machete` green; `changes` failed on GitHub diff API transient and dependent jobs skipped. Local clippy evidence already posted. No exact competing PR found. | `PR-OPEN / review-rerun-watch` | Wait maintainer review or workflow rerun. |
| [OpenHands/software-agent-sdk#3394](https://github.com/OpenHands/software-agent-sdk/pull/3394) | PR open; maintainer said it looks fine but wants eval. Eval was triggered; check rollup currently empty and GitHub still shows stale `CHANGES_REQUESTED` from bot review. No exact competing PR found. | `PR-OPEN / eval-watch` | Wait maintainer eval result or review refresh. |
| [pydantic/pydantic-ai#5688](https://github.com/pydantic/pydantic-ai/issues/5688) | Regression card and local patch prep remain strong, but `pydanty` already marked root cause PR-ready/delegated. No exact PR found in this live pass. We already have two active pydantic-ai PRs waiting review. | `WATCH / assignment-first` | Wait maintainer assignment/explicit confirmation or delegated PR appearance. |
| [google/adk-python#5864](https://github.com/google/adk-python/issues/5864) | Issue still assigned and labelled `request clarification`; local import-boundary patch prep exists. | `WATCH / clarification-gated` | Wait maintainer/reporter direction. |
| [langchain-ai/langgraph#7688](https://github.com/langchain-ai/langgraph/issues/7688) | Repro evidence exists, but affected package surface is still unclear; our evidence comment is minimized. | `WATCH / package-surface direction` | Wait maintainer confirmation of public patch surface. |

## Parent Gate: MCP TypeScript SDK #2115

Live correction to cycle 21:

- Issue: https://github.com/modelcontextprotocol/typescript-sdk/issues/2115
- State: open.
- Assignee: none.
- Labels: none.
- Our upstream test-plan comment: https://github.com/modelcontextprotocol/typescript-sdk/issues/2115#issuecomment-4554920638
- Exact competing PRs:
  - https://github.com/modelcontextprotocol/typescript-sdk/pull/2116
  - https://github.com/modelcontextprotocol/typescript-sdk/pull/2138
- Both PRs:
  - are open, mergeable, review required;
  - have green checks in the live view;
  - change `packages/core/src/shared/protocol.ts`;
  - add regression coverage in `packages/core/test/shared/protocol.test.ts`;
  - add a changeset for `@modelcontextprotocol/core`;
  - cover the reported `requestId: 0` cancellation bug.

Correction:

- Cycle 21 said "duplicate gate clean"; that is now stale.
- The upstream comment-first note remains historical evidence only. It is not a current PR permission signal.
- Current status is `WATCH / duplicate-race`, not `COMMENT-FIRST / candidate`.

## Critique

Fact-check:

- Status change is factual: `#2116` and `#2138` are exact duplicate PRs for `#2115`.
- Do not claim these PRs cover `null` request ids; the reported issue is `requestId: 0`, and duplicate coverage is enough for that contract.

Process-gate:

- Required sequence was followed: 6-subagent scouting, parent live gates, 3-subagent critique, then tracker/watch update.
- No external PR/comment was opened.
- Runner/test step was not needed because the cycle stops on duplicate gate and does not enter patch/PR workflow.

Actionability:

- `next_status`: `WATCH`.
- `unblock_event`: maintainer merges/closes `#2116`/`#2138` or explicitly asks for an alternative fix/tests.
- Before any future upstream action, repeat duplicate gate over linked PRs and PR search.

## Decision

No new upstream PR or comment from this cycle. The only tracker status change is `modelcontextprotocol/typescript-sdk#2115` from `COMMENT-FIRST / candidate` to `WATCH / duplicate-race`.

## Follow-up 23:11 UTC

Bounded duplicate-race refresh for `modelcontextprotocol/typescript-sdk#2115`: 6 read-only subagents, parent live gates via `gh`, then 3-role critique. No upstream PR/comment/ping was made.

Tracker update: [#51 comment](https://github.com/serejaris/corp-opensource/issues/51#issuecomment-4559436572).

Live gates:

- Issue `#2115`: open, unassigned, no labels. Our upstream test-plan comment from `2026-05-27T13:25:46Z` remains the latest non-minimized substantive comment.
- Exact PR `#2116`: open, non-draft, `MERGEABLE/BLOCKED/REVIEW_REQUIRED`; visible checks green; changes `packages/core/src/shared/protocol.ts`, adds a changeset for `@modelcontextprotocol/core`, and adds regression coverage in `packages/core/test/shared/protocol.test.ts`; updated `2026-05-25T18:21:41Z`.
- Exact PR `#2138`: open, non-draft, `MERGEABLE/BLOCKED/REVIEW_REQUIRED`; visible checks green; changes `packages/core/src/shared/protocol.ts`, adds a changeset for `@modelcontextprotocol/core`, and adds regression coverage in `packages/core/test/shared/protocol.test.ts`; updated `2026-05-27T10:47:01Z` after rebase and broader local validation notes.
- Adjacent cancellation/request PRs such as `#1932`, `#2141`, and `#2028` are context only; `#2116/#2138` remain the exact duplicate coverage for the `requestId: 0` cancel path.

6-subagent synthesis:

- Factology: `WATCH`, not `COMMENT-FIRST` or `PR-READY`; do not claim either PR is merged, approved, or selected by maintainers.
- Duplicate/race: direct coverage is high; a third PR would be duplicate noise.
- Process: our historical upstream comment already covered comment-first; no maintainer assignment/direction followed.
- PR comparison: `#2138` is the freshest duplicate lane / primary watch target by update time and validation envelope, but not an upstream-selected winner.
- Runner/repro: no runner needed for this internal duplicate-race refresh; exact competing PRs already have tests, changesets, and green checks.
- Final synthesis: `next_status: WATCH`; wait maintainer review/selection/merge/close.

3-role critique:

- Factology/duplicates: approved with wording guardrail: use "freshest duplicate lane", not "best" or "selected".
- Process: approved. This is tracker-only; no upstream action, no runner, no PR workflow.
- Actionability: approved. Re-open actionability only if both exact PRs close unmerged, maintainer asks for evidence/implementation, CI regresses, or a new uncovered edge appears outside the current `requestId: 0` path.

Decision remains `next_status: WATCH`.

## Follow-up 23:37 UTC

Bounded duplicate-race heartbeat for `modelcontextprotocol/typescript-sdk#2115`: 6 read-only subagents, parent live gates via `gh`, then 3-role critique. No upstream PR/comment/ping was made.

Tracker update: [#51 comment](https://github.com/serejaris/corp-opensource/issues/51#issuecomment-4559578452).

Live gates:

- Issue `#2115`: still open, unassigned, no labels; no maintainer signal after our historical test-plan comment from `2026-05-27T13:25:46Z`.
- Exact PR `#2116`: open, non-draft, `MERGEABLE/BLOCKED/REVIEW_REQUIRED`; visible checks green; patch changeset for `@modelcontextprotocol/core`; regression coverage for `requestId: 0` in `packages/core/test/shared/protocol.test.ts`; latest commit `0d4e4115ce03381a8228c3a921bde0648d4f7de9`.
- Exact PR `#2138`: open, non-draft, `MERGEABLE/BLOCKED/REVIEW_REQUIRED`; visible checks green; patch changeset for `@modelcontextprotocol/core`; regression coverage for `requestId: 0`; latest commit `553b6b696fc5f59b6b930aeba1368a3a48e4e99f`; freshest duplicate lane by update/comment time, not an upstream-selected winner.
- Adjacent `relatedRequestId: 0` / debounce PRs `#2120`, `#2135`, and `#2141` remain separate `#2117` context, not a new uncovered `#2115` gap.

6-subagent synthesis:

- Factology: `#2115` bug shape remains valid, but comment-first already happened and maintainer direction is absent.
- Duplicate/race: `#2116` and `#2138` both cover the same `_oncancel` / `requestId: 0` contract with tests, changesets, and green CI.
- Process: a third PR or repeated upstream comment would be duplicate noise.
- Actionability: runner/repro is not needed for this heartbeat because the gate is upstream selection/review, not missing local evidence.
- Final synthesis: `next_status: WATCH / duplicate-race`; `upstream_action_count: 0`.

3-role critique:

- Factology/duplicates: approved; adjacent `relatedRequestId: 0` lanes are separate and do not reopen `#2115`.
- Process gates: approved for internal tracker/watch update only; no upstream comment, PR, or ping.
- Actionability: approved. Re-open only if both exact PRs close unmerged, maintainers ask for another approach/evidence, relevant CI regresses, a new uncovered edge appears, or one exact PR merges and the internal watch should close as absorbed.

Decision remains `next_status: WATCH`.
