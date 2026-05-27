# opencode #29428 - Bedrock/LiteLLM Task subagent tools

Date: 2026-05-27

## Upstream

- Issue: https://github.com/anomalyco/opencode/issues/29428
- Closed attempt: https://github.com/anomalyco/opencode/pull/29474

## Subagent Synthesis

Verdict: real issue class, but no clean PR lane right now.

`#29428` is not a clean duplicate of older issues `#2915`, `#10037`, or `#10259`. Those mostly covered compaction/history-with-tools and LiteLLM proxy behavior. This issue is specifically Task subagents + Bedrock through LiteLLM + MCP/tool history.

However, PR `#29474` already attempted the likely patch and was closed unmerged by the maintainer on 2026-05-27 with no public explanation. A fresh PR that only re-adds `_noop` compatibility for Bedrock/LiteLLM would likely be duplicate churn.

## Technical Read

Likely code path:

- `packages/opencode/src/tool/task.ts`: `TaskTool.runTask()` creates/resumes child session and passes a disable-map for `todowrite`, nested `task`, and primary tools. It does not directly drop MCP tools.
- `packages/opencode/src/session/tools.ts`: MCP tools are added into the session tool map.
- `packages/opencode/src/session/llm/request.ts`: `LLMRequestPrep.prepare()` filters tools and currently injects `_noop` only for `github-copilot` when history contains tool calls but resolved tools are empty.
- `packages/opencode/src/session/compaction.ts`: compaction calls the processor with `tools: {}`, making a no-tools-with-tool-history request plausible.

## Repro Shape

Credentials are not required. A request-shape test can mock an OpenAI-compatible/LiteLLM endpoint:

- provider model shaped like Bedrock through LiteLLM;
- messages containing prior `tool-call` / `tool-result`;
- resolved `tools: {}`;
- assert whether the outgoing body includes a compatibility `tools` payload.

## PR Risk

Small patch is possible, but maintainer risk is high because:

- opencode previously removed LiteLLM-specific workaround logic after LiteLLM upstream changes;
- `#29474` already added a similar regression test and compatibility behavior;
- `_noop` tools can be undesirable if the model calls them;
- a broad provider heuristic may be rejected as a LiteLLM config issue.

## Next Action

Ask upstream why `#29474` was closed and what shape would be acceptable:

- opt-in via `litellmProxy`;
- narrower Bedrock-only detection;
- documentation recommending LiteLLM `modify_params`;
- or no opencode-side workaround.

Until maintainer signal arrives: no fresh PR.
