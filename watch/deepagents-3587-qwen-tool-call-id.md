# deepagents#3587 - Qwen-compatible subagent task loses tool_call_id

Date: 2026-05-27

## Links

- Upstream issue: https://github.com/langchain-ai/deepagents/issues/3587
- Internal issue: https://github.com/serejaris/corp-opensource/issues/20
- Local checkout: `/Users/ris/Documents/GitHub/deepagents-3587`

## Status

`CANDIDATE / NEEDS-REPRO`.

No open or closed competing PR was found via `gh search prs` for:

- `3587`
- `Tool call ID is required`
- `Qwen`

## Bug

The reporter says OpenAI-compatible Qwen models can call normal tools, but fail when the built-in `task` subagent tool runs:

```text
ValueError: Tool call ID is required for subagent invocation
```

The current code raises this in `deepagents/middleware/subagents.py` for both sync and async task paths when `runtime.tool_call_id` is missing.

## Why It Matters

This is directly in the target lane:

- subagent orchestration;
- OpenAI-compatible model adapters;
- tool-call streaming/runtime contract;
- Paperclip/Hermes-like worker delegation reliability.

## Risk

Do not implement a broad fallback in `task` just to suppress the error.

Without a `tool_call_id`, the returned `ToolMessage` cannot obviously be matched to the model's original tool call. The correct fix depends on where the ID is lost:

- model stream chunk;
- LangChain message conversion;
- ToolRuntime construction;
- Deep Agents wrapper.

## Next Action

Build a synthetic repro without a real Qwen/API key:

1. Use the existing fake model test surface in `libs/deepagents/tests/unit_tests/`.
2. Simulate the OpenAI-compatible streamed tool call shape that drops or omits the id.
3. Confirm current `main` reaches the same `ValueError`.
4. Only then decide whether the fix belongs in Deep Agents or an upstream adapter layer.

Repo rules:

- PR title must follow `TYPE(SCOPE): DESCRIPTION`, for example `fix(deepagents): ...`.
- PR body starts with `Fixes #3587`.
- From `libs/deepagents`, run `make format`, `make lint`, and `make test` before PR.
