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
