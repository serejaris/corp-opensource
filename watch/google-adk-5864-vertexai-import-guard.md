# google/adk-python #5864: Vertex AI optional dependency import guard

Date: 2026-05-27

## Upstream

- Issue: https://github.com/google/adk-python/issues/5864
- Internal tracker: https://github.com/serejaris/corp-opensource/issues/46
- Our offer comment: https://github.com/google/adk-python/issues/5864#issuecomment-4553922296
- State checked on 2026-05-27: issue is open, assigned to `surajksharma07`, labels `core` and `request clarification`.
- Duplicate scan checked on 2026-05-27: no PR found for `#5864`, `vertexai`, `google-adk[gcp]`, or `dependencies/vertexai.py`.

## Decision

No upstream PR yet.

Reason: maintainer suggested the shape but asked the reporter to validate and run a full test pass. The issue is assigned and clarification-gated, so opening a drive-by PR would race the repo process.

## Local patch prep

- Local checkout: `/Users/ris/Documents/GitHub/adk-python-5864`
- Branch: `codex/5864-vertexai-import-guard`
- Local commit: `70820eea fix(dependencies): Explain missing Vertex AI extra`
- Patch surface: `src/google/adk/dependencies/vertexai.py`
- Test surface: `tests/unittests/test_optional_dependencies.py`

## Regression card

- Contract: importing `google.adk.dependencies.vertexai` without the Vertex AI optional dependency should raise a clear install-extra hint, not a raw missing-module error.
- Test file: `tests/unittests/test_optional_dependencies.py`
- Test name: `test_vertexai_dependency_shim_fails_with_install_extra_hint`
- Command:

```bash
uv run python -m pytest tests/unittests/test_optional_dependencies.py -k vertexai_dependency_shim
```

- Pre-fix failure:

```text
AssertionError: assert 'google-adk[gcp]' in 'import of vertexai halted; None in sys.modules'
```

- Post-fix result:

```text
1 passed, 11 deselected in 1.12s
```

## Verification

```bash
uv sync --extra test --extra dev
uv run python -m pytest tests/unittests/test_optional_dependencies.py -k vertexai_dependency_shim
uv run python -m pytest tests/unittests/test_optional_dependencies.py
uv run pyink --check src/google/adk/dependencies/vertexai.py tests/unittests/test_optional_dependencies.py
uv run isort --check-only src/google/adk/dependencies/vertexai.py tests/unittests/test_optional_dependencies.py
```

Results:

- targeted pytest: `1 passed, 11 deselected`
- full optional dependency file: `4 passed, 8 skipped`
- `pyink --check`: unchanged
- `isort --check-only`: passed

Full `tox` was not run locally because this is still a gated local patch prep, not an upstream PR.

## Test lesson

This is the right shape for optional dependency bugs:

1. Force the public import boundary that users hit.
2. Assert the clear install-extra message.
3. Prove the test fails before the patch.
4. Patch only the import shim.

A helper-level predicate test would be too weak here because the bug is the raw import boundary.
