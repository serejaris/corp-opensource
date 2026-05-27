# deepagents#3587 - Qwen-compatible subagent task loses tool_call_id

Date: 2026-05-27

## Links

- Upstream issue: https://github.com/langchain-ai/deepagents/issues/3587
- Upstream PR: https://github.com/langchain-ai/deepagents/pull/3616
- Internal issue: https://github.com/serejaris/corp-opensource/issues/20
- Local checkout: `/Users/ris/Documents/GitHub/deepagents-3587`

## Status

`PR AUTO-CLOSED BY ASSIGNMENT GUARD / WAITING MAINTAINER`.

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

## Fix

Opened PR #3616 from branch `serejaris:codex/repro-3587-tool-call-id`.

Root cause narrowed to empty-string tool call ids:

- a normal LangGraph tool call with `id=""` works;
- Deep Agents `task` rejected the same id because it used truthiness: `if not runtime.tool_call_id`;
- `None` still means genuinely missing and should keep raising.

The PR changes the sync and async task paths to reject only `None`, preserving `""` as the tool call id.

## Verification

Pre-fix regression proof:

```bash
uv run --group test pytest tests/unit_tests/test_subagents.py -k empty_string_tool_call_id --no-cov
```

Failed with:

```text
ValueError: Tool call ID is required for subagent invocation
```

Post-fix checks from `libs/deepagents`:

```bash
uv run --group test pytest tests/unit_tests/test_subagents.py -k 'empty_string_tool_call_id' --no-cov
make test TEST_FILE=tests/unit_tests/test_subagents.py
make format
make lint
make test
```

Results:

- focused regression: `2 passed`;
- owner test file: `33 passed, 1 xfailed`;
- full unit suite: `1620 passed, 90 skipped, 4 xfailed`;
- format/lint/type: passed.

## PR State

Checked after opening:

- state: closed by automation;
- reason: external author is not assigned to linked issue;
- title lint: green;
- lockfile check: green;
- local tests: green;
- Corridor Review: green;
- CI jobs were cancelled after the assignment guard closed the PR.

## Next Action

Wait for maintainer response on issue #3587.

Posted upstream issue comment:

https://github.com/langchain-ai/deepagents/issues/3587#issuecomment-4551838560

The comment explains the root cause, why `None` should still raise, the local verification, and asks maintainers to confirm whether this direction matches the intended contract.

## Original Next Action

Build a synthetic repro without a real Qwen/API key:

1. Use the existing fake model test surface in `libs/deepagents/tests/unit_tests/`.
2. Simulate the OpenAI-compatible streamed tool call shape that drops or omits the id.
3. Confirm current `main` reaches the same `ValueError`.
4. Only then decide whether the fix belongs in Deep Agents or an upstream adapter layer.

Repo rules:

- PR title must follow `TYPE(SCOPE): DESCRIPTION`, for example `fix(deepagents): ...`.
- PR body starts with `Fixes #3587`.
- From `libs/deepagents`, run `make format`, `make lint`, and `make test` before PR.
