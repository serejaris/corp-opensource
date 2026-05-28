# Pydantic AI serialize_any Binary Data Watch

## Upstream

- Issue: https://github.com/pydantic/pydantic-ai/issues/5666
- PR: https://github.com/pydantic/pydantic-ai/pull/5678
- Local checkout: `/Users/ris/Documents/GitHub/pydantic-ai-5666`
- Branch: `codex/serialize-any-bytes`
- Commit: `eec87ea0 fix: preserve binary data in instrumentation serialization`

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
- `docs`: passed.
- PR bots category/size/apply: passed.
- examples on Python 3.11/3.12/3.13/3.14: passed.
- `pydantic-ai-slim` tests on Python 3.10/3.11/3.12/3.13/3.14: passed.
- `pydantic-evals` tests on Python 3.10/3.11/3.12/3.13/3.14: passed.
- broader Python matrix still pending.
- `chatgpt-codex-connector` posted that code review usage limits were reached; no code action required.

CI repair:

- First CI `lint` job failed inside the pre-commit `typecheck` hook, not Ruff.
- Cause: Pyright reported partially unknown types in `_json_safe_bytes()` for `Mapping`, `tuple`, and `list` iterations.
- Fix: added local casts before iterating those containers.
- New local validation:
  - `uv run pyright pydantic_ai_slim/pydantic_ai/_instrumentation.py` -> `0 errors, 0 warnings, 0 informations`
  - `uv run pytest tests/models/test_instrumented.py -q -k 'serialize_any_preserves_binary_data or messages_to_otel_events_serialization_errors'` -> `2 passed`
  - `uv run ruff check ...` and `uv run ruff format --check ...` -> passed
- Amended and force-pushed branch to commit `be323816`.
- New CI run started; checks are pending again.

Third check:

- New CI run for `be323816` is still in progress.
- Passed after the Pyright fix:
  - `lint`
  - `mypy`
  - `docs`
  - `harness compat`
  - `pydantic-ai-slim` tests on Python 3.10/3.11/3.12/3.13/3.14
  - `pydantic-evals` tests on Python 3.10/3.11/3.12/3.13/3.14
  - examples on Python 3.11/3.12/3.13/3.14
  - standard tests on Python 3.10/3.11/3.12/3.13/3.14
  - all-extras tests on Python 3.14
  - lowest-versions tests on Python 3.14
- Still pending: remaining all-extras and lowest-versions matrix.
- Do not open another upstream PR until this CI result is known.

Fourth check:

- All completed checks remain green.
- Newly confirmed green:
  - all standard tests on Python 3.10/3.11/3.12/3.13/3.14
  - all-extras on Python 3.13/3.14
  - lowest-versions on Python 3.12/3.14
- Still pending:
  - all-extras on Python 3.10/3.11/3.12
  - lowest-versions on Python 3.10/3.11/3.13
- No upstream comment needed while CI is still running and no maintainer action is requested.

Coverage repair:

- The full test matrix passed, then the final coverage job failed because total coverage was `99.99`, below `fail-under=100.00`.
- Missing lines were `_json_safe_bytes()` tuple handling (`_instrumentation.py:105-106`).
- Added a minimal assertion covering tuple/list nested bytes:
  - `serialize_any((b'\xff', [b'\x00'])) == ['/w==', ['AA==']]`
- Local validation after repair:
  - targeted `test_instrumented.py` selection -> `2 passed`
  - full `tests/models/test_instrumented.py` -> `45 passed`
  - `pyright` on `_instrumentation.py` -> `0 errors`
  - `ruff check` and `ruff format --check` -> passed
- Amended and force-pushed branch to commit `eec87ea0`.
- New CI run started; wait for fresh result.

Fifth check, 2026-05-27 01:32 -03:

- Fresh CI run is for commit `eec87ea0`.
- Confirmed green so far:
  - `mypy`
  - `harness compat`
  - PR bots category/size
  - examples on Python 3.11/3.12/3.13/3.14
  - `pydantic-ai-slim` tests on Python 3.10/3.11/3.12/3.13/3.14
  - `pydantic-evals` tests on Python 3.10/3.11/3.12/3.13/3.14
- Still pending:
  - `lint`
  - `docs`
  - standard/all-extras/lowest-versions matrix
- No upstream comment posted; nothing is actionable for maintainers while the fresh run is still pending.

Process lesson captured:

- Updated local `open-source-pr-workflow` skill and `corp-opensource` follow-up playbook: when upstream has strict coverage gates, targeted `-k` tests are not enough before push. Run the smallest full owner test file/module and cover every new branch/data shape touched by the patch.

Final CI check, 2026-05-27 01:41 -03:

- Fresh CI for commit `eec87ea0` is green.
- Confirmed passed:
  - `lint`
  - `mypy`
  - `docs`
  - `coverage`
  - summary `check`
  - harness compat
  - examples on Python 3.11/3.12/3.13/3.14
  - `pydantic-ai-slim`, `pydantic-evals`, `standard`, `all-extras`, and `lowest-versions` matrix on Python 3.10/3.11/3.12/3.13/3.14
- Skipped jobs are release/deploy/review automation and do not require code action.
- PR remains open and mergeable; next action is maintainer/bot review monitoring.

## Next

Watch maintainer / bot review. If maintainers prefer an explicit wrapper shape for binary values instead of raw base64 strings, adapt the branch.

## Dashboard heartbeat - 2026-05-28 01:15 UTC

- PR `#5678` remains open, non-draft, `MERGEABLE/CLEAN`.
- Visible lint, mypy, docs, test matrix, examples, coverage, check, harness compat, and smokeshow contexts are green.
- No maintainer review yet; Codex review quota warning remains non-actionable.
- Upstream action count: `0`.
- Next action: wait maintainer/bot review.
