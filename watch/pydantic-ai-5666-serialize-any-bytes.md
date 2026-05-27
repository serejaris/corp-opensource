# Pydantic AI serialize_any Binary Data Watch

## Upstream

- Issue: https://github.com/pydantic/pydantic-ai/issues/5666
- PR: https://github.com/pydantic/pydantic-ai/pull/5678
- Local checkout: `/Users/ris/Documents/GitHub/pydantic-ai-5666`
- Branch: `codex/serialize-any-bytes`
- Commit: `9e712842 fix: preserve binary data in instrumentation serialization`

## Why In Scope

`pydantic/pydantic-ai` is a Tier 1 repo in the AI-native framework scope. This bug affects instrumentation / observability data integrity for tool arguments and tool results, which is relevant to agent harness debugging.

## Duplicate Check

- `pydantic-ai#5629` had an existing competing PR, `pydantic-ai#5630`, so it was skipped as a fresh PR target.
- For `pydantic-ai#5666`, searched open PRs for `serialize_any`, `bytes`, and OTel instrumentation before opening a PR. No competing PR was found.

## Repro

Before the patch:

```text
serialize_any(b'\xff') -> "b'\\xff'"
serialize_any({'data': b'\xff'}) -> "{'data': b'\\xff'}"
```

That loses structured binary data and can stringify the whole object.

## Patch

Adds a fallback pass in `pydantic_ai_slim/pydantic_ai/_instrumentation.py` that converts bytes to base64 before the final `str()` fallback, including bytes nested in mappings, lists, and tuples.

## Validation

```text
uv run pytest tests/models/test_instrumented.py -q -k 'serialize_any_preserves_binary_data or messages_to_otel_events_serialization_errors'
2 passed

uv run pytest tests/models/test_instrumented.py -q
45 passed

uv run ruff format --check pydantic_ai_slim/pydantic_ai/_instrumentation.py tests/models/test_instrumented.py
2 files already formatted

uv run ruff check pydantic_ai_slim/pydantic_ai/_instrumentation.py tests/models/test_instrumented.py
All checks passed!
```

## Initial Upstream State

PR `pydantic-ai#5678` opened on 2026-05-27. Initial state: open, ready, mergeable. CI started; checks were pending at first check.

Second check:

- `mypy`: passed.
- `Guard`: passed.
- `harness compat`: passed.
- PR bots category/size/apply: passed.
- several example tests already passed.
- broader Python matrix still pending.
- `chatgpt-codex-connector` posted that code review usage limits were reached; no code action required.

## Next

Watch CI and maintainer / bot review. If maintainers prefer an explicit wrapper shape for binary values instead of raw base64 strings, adapt the branch.
