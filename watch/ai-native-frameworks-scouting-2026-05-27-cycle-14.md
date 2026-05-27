# AI-native frameworks scouting: cycle 14

Date: 2026-05-27
Mode: 6-subagent scouting + 3-subagent PR-readiness check for the only fresh candidate.

## Outcome

Safe new ready PR: none yet.

Best fresh candidate: `google/adk-python#5864`, but current action is comment-first/watch, not immediate PR.

Update: local patch prep exists, but upstream PR is still intentionally blocked by the assignment/clarification gate.

## Upstream state

- Issue: https://github.com/google/adk-python/issues/5864
- Internal tracker: https://github.com/serejaris/corp-opensource/issues/46
- Comment-offer posted: https://github.com/google/adk-python/issues/5864#issuecomment-4553922296
- Status: open; labels `core`, `request clarification`; assigned to `surajksharma07`.
- Maintainer signal: collaborator suggested wrapping the three imports in `google/adk/dependencies/vertexai.py` with `try/except ImportError`, but addressed the reporter and asked them to validate and run a full test pass.
- Duplicate scan: no open/closed PR found for `#5864`, `dependencies/vertexai.py`, `vertexai` optional dependency, or `google-adk[gcp]` / `google-adk[all]` missing-extra guard.
- Local patch prep: [google-adk-5864-vertexai-import-guard.md](google-adk-5864-vertexai-import-guard.md), branch `codex/5864-vertexai-import-guard`, commit `70820eea`.

## Regression card

- User contract: a default install / environment without `vertexai` should not surface a raw `ModuleNotFoundError` when `google.adk.dependencies.vertexai` is touched. It should raise a clear install-extra message for `google-adk[gcp]` or `google-adk[all]`.
- Patch surface: `src/google/adk/dependencies/vertexai.py`.
- Test surface: `tests/unittests/`; likely an optional-dependency/import-guard regression.
- Pre-fix proof from current `main`:

```bash
python3 - <<'PY'
import importlib.util

spec = importlib.util.spec_from_file_location(
    "adk_vertexai_dep", "src/google/adk/dependencies/vertexai.py"
)
mod = importlib.util.module_from_spec(spec)
spec.loader.exec_module(mod)
PY
```

Observed failure:

```text
ModuleNotFoundError: No module named 'vertexai'
```

## PR-readiness notes

- Repo contribution guide says to ask before contributing on issues that are not `good first issue` or `help wanted`.
- `#5864` is not labeled `help wanted`; it has `request clarification`.
- PRs require issue link, focused scope, testing plan, unit test evidence, and manual/E2E evidence where relevant.
- Google CLA is required. No DCO/signoff requirement found.
- Conventional Commit style is expected by repo agent docs.
- Required-before-PR test in docs: `tox`; targeted `pytest` is still needed during development.

## Decision

Do not open a drive-by PR while the maintainer is waiting on reporter validation.

Next action:

1. Watch for maintainer/reporter reply.
2. If greenlit, create `fix/5864-vertexai-import-error`.
3. Add regression-first test for missing `vertexai`.
4. Patch only `src/google/adk/dependencies/vertexai.py`.
5. Run targeted pytest and record whether full `tox` was run locally or deferred because of runtime cost.

## Local patch prep result

- Pre-fix regression was proved through public import boundary:

```text
AssertionError: assert 'google-adk[gcp]' in 'import of vertexai halted; None in sys.modules'
```

- Post-fix verification:

```text
uv run python -m pytest tests/unittests/test_optional_dependencies.py -k vertexai_dependency_shim
# 1 passed, 11 deselected

uv run python -m pytest tests/unittests/test_optional_dependencies.py
# 4 passed, 8 skipped

uv run pyink --check src/google/adk/dependencies/vertexai.py tests/unittests/test_optional_dependencies.py
# unchanged

uv run isort --check-only src/google/adk/dependencies/vertexai.py tests/unittests/test_optional_dependencies.py
# passed
```

## Lesson

For optional dependency import bugs, the regression must assert the public missing-extra/error contract. A test that only checks a helper predicate is too weak: the broken behavior is the import boundary users hit.

For assignment/clarification-gated repos, a clean regression card is not enough to skip social protocol. Comment first, link the exact test plan, then wait for maintainer direction.
