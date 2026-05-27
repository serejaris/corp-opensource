# AI-native Frameworks Scouting Cycle 21

Checked at `2026-05-27T13:19Z`.

## Method

Six-subagent scouting pass:

1. repo-fit;
2. bug-signal;
3. repro-path;
4. patchability;
5. duplicate-race;
6. PR-readiness.

Parent gate then rechecked the only new candidate that was not clearly duplicate-covered or assignment-gated.

## Shortlist

| Target | Subagent signal | Parent decision | Why | Next action |
|---|---|---|---|---|
| [modelcontextprotocol/typescript-sdk#2115](https://github.com/modelcontextprotocol/typescript-sdk/issues/2115) `requestId: 0` cancel notification ignored | Duplicate-race found no open PR, no assignee, relevant MCP cancellation/resource leak | `COMMENT-FIRST / candidate` | Issue is open and unassigned; PR search returned empty. But it does not have `ready for work`; repo asks contributors to comment before starting work. | Post upstream test-plan comment; prepare only after maintainer confirmation or if issue is labelled/assigned. |
| [microsoft/playwright-mcp#1635](https://github.com/microsoft/playwright-mcp/issues/1635) `browser_navigate_back` timeout on CDP attach | Bug-signal and PR-readiness both strong | `WATCH / maintainer-direction-needed` | No competing PR found, but meaningful code belongs in Playwright monorepo MCP suite and contribution guidance is approval/assignment-first. We already posted a regression offer. | Wait maintainer direction. |
| [pydantic-ai#5688](https://github.com/pydantic/pydantic-ai/issues/5688) `MCPToolset(url, http_client=...)` crashes on `follow_redirects` | Repo-fit, repro-path, patchability all strong | `WATCH / assignment-first` | Tiny patch, but `pydanty` already localized root cause and says PR-ready; repo has assignment/delegation expectations. | Wait maintainer confirmation/assignment or pydanty PR. |
| [openai-agents-python#3512](https://github.com/openai/openai-agents-python/issues/3512) `RunHooksBase.on_tool_end` loses non-string result | Repro-path found plausible no-secret unit test | `WATCH / needs duplicate gate` | Good fresh signal, but not enough repo rules / duplicate-race evidence in this cycle. | Fresh parent gate if selected later. |
| [browser-use#4846](https://github.com/browser-use/browser-use/issues/4846) MCP CDP tools fail while navigate works | Repo-fit/bug-signal strong | `NO-GO / duplicate-covered` | Linked open PRs `#4847` and `#4881` already cover adjacent/exact behavior. | Watch existing PRs only. |
| [cline#10737](https://github.com/cline/cline/issues/10737) MCP `task_progress` leaks into tool args | Patchability small | `WATCH / duplicate-race` | Old stale/conflicting PR cluster exists; not clean enough for fresh PR. | Wait maintainer signal. |

## Parent Gate: MCP TypeScript SDK #2115

Live check:

- Issue: https://github.com/modelcontextprotocol/typescript-sdk/issues/2115
- State: open.
- Assignee: none.
- Labels: none.
- PR search: `2115 OR _oncancel OR oncancel OR requestId 0 OR cancellation resource leak` -> no results.
- Contribution rule: straightforward bug fixes can be small PRs, but issues without readiness labels should still get a comment to avoid duplicate work.

Regression card:

- User-facing contract: cancellation notifications must abort the matching in-flight request even when `requestId` is `0`.
- Expected pre-fix failure: a cancellation notification with `params.requestId = 0` does not abort the stored controller.
- Expected post-fix pass: the controller for request id `0` is aborted with the provided reason.
- Likely test surface: `packages/core/src/shared/protocol.ts` tests around `Protocol` cancellation handling.
- Likely patch surface: `_oncancel` should reject only `undefined` / `null` request ids, not all falsy ids.

Decision:

- `COMMENT-FIRST / candidate`.
- Do not open PR before upstream comment/maintainer signal unless this becomes clearly accepted as a straightforward bugfix in the thread.
