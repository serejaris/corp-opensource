# Pydantic AI Google cached content watch

## Upstream

- Issue: https://github.com/pydantic/pydantic-ai/issues/5671
- Competing PR: https://github.com/pydantic/pydantic-ai/pull/5681
- Repo: `pydantic/pydantic-ai`
- Status checked: 2026-05-27 02:13 -03

## Signal

`GoogleModelSettings.google_cached_content` is documented, but current issue reports Vertex/Gemini rejects requests when `cached_content` is combined with `system_instruction`, `tools`, or `tool_config`.

The bug is in scope because it is a provider adapter request-shape bug in a typed agent framework.

## Duplicate / PR State

Subagents found this as the cleanest unit-testable candidate, but a competing PR appeared before implementation:

- PR `#5681` by `gaurav0107`.
- State: open, ready, mergeable.
- CI: lint, mypy, docs, matrix tests, coverage, and summary `check` all green.
- Files: `pydantic_ai_slim/pydantic_ai/models/google.py`, `tests/models/test_google.py`.
- PR closes `#5671`.

Patch shape in `#5681`:

- stores `cached_content = model_settings.get('google_cached_content')`;
- sets `system_instruction`, `tools`, and `tool_config` to `None` when cached content is set;
- adds tests around patched `generate_content` to assert cache-owned fields are unset.

## Decision

`WATCH / review only`: do not open a competing PR. Useful next actions:

- watch maintainer review;
- comment only if review finds a concrete missing behavior;
- record final outcome when merged/closed.

## Lesson

This is the exact failure mode the workflow now guards against: a candidate can become occupied during research. Run duplicate search again immediately before checkout/branch work, not only at the start of scouting.
