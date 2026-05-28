# Pydantic AI TextContent Metadata Round-trip Watch

## Upstream

- Issue: https://github.com/pydantic/pydantic-ai/issues/5679
- PR: https://github.com/pydantic/pydantic-ai/pull/5680
- Internal issue: https://github.com/serejaris/corp-opensource/issues/16
- Repo: `pydantic/pydantic-ai`
- Status: upstream PR open, mergeable, CI green
- Branch: `codex/textcontent-metadata-roundtrip`
- Local checkout: `/Users/ris/Documents/GitHub/pydantic-ai-5679`
- Commit: `fec6e534 fix: preserve TextContent metadata in UI adapters`

## Why In Scope

This is directly in the Paperclip/Hermes/Codex harness lane: message history round-trip loses protocol metadata for MCP-origin text content. That affects resumed agent conversations, source attribution, and UI adapter correctness.

## Bug Signal

The issue reports that `TextContent(metadata=...)` attached by MCP paths is flattened to plain `str` when messages are dumped and reloaded through UI adapters.

Affected paths from the issue:

- `pydantic_ai/ui/vercel_ai/_adapter.py`
- `pydantic_ai/ui/ag_ui/_adapter.py`

The reproduction is precise and has three confirmed failing pytest cases in the issue body.

## Duplicate Check

Checked on 2026-05-27 01:38 -03:

```text
gh search prs --repo pydantic/pydantic-ai "5679 OR metadata OR TextContent.metadata OR ag_ui OR vercel_ai" --state open
[]
```

Related older PR `pydantic-ai#3339` is about MCP result metadata, not this UI adapter round-trip path.

## Repro / Test Plan

First local command:

```bash
uv run pytest tests/test_vercel_ai.py tests/test_ag_ui.py -q
```

Expected regression shape:

- Vercel AI adapter dump/load preserves `TextContent(content=..., metadata=...)`.
- AG-UI adapter conversion preserves metadata or has an explicit structured representation.
- Tests cover plain `str` alongside `TextContent` so the adapter does not wrap every string unnecessarily.

## PR Readiness

Ready only after current `pydantic-ai#5678` is not pending anymore.

Before implementation:

- re-run duplicate PR search;
- read current adapter tests;
- add failing tests first;
- run owner test files, Pyright, Ruff, and coverage-aware checks;
- avoid broad adapter redesign.

## Decision

`PR opened`: strongest next pydantic candidate because it has exact code paths, no open duplicate PR found, and a narrow testable patch surface.

## Implementation

Opened upstream PR `pydantic-ai#5680` on 2026-05-27.

Patch:

- Vercel AI adapter stores `TextContent.metadata` in `TextUIPart.provider_metadata.pydantic_ai.text_content_metadata` and restores `TextContent` on load when that marker is present.
- AG-UI adapter stores metadata in a Pydantic AI extra field on `TextInputContent`, `pydantic_ai_text_content_metadata`, and avoids collapsing that part to a plain string.
- Plain strings and `TextContent(metadata=None)` keep the previous plain-string behavior.

TDD evidence:

- New Vercel AI and AG-UI regression tests failed before the fix because `TextContent` reloaded as `str`.
- The same tests passed after the fix.

Local validation:

```text
uv run pytest tests/test_vercel_ai.py tests/test_ag_ui.py -q
272 passed

PYRIGHT_PYTHON_IGNORE_WARNINGS=1 uv run pyright pydantic_ai_slim/pydantic_ai/ui/vercel_ai/_adapter.py pydantic_ai_slim/pydantic_ai/ui/ag_ui/_adapter.py tests/test_vercel_ai.py tests/test_ag_ui.py
0 errors, 0 warnings, 0 informations

uv run ruff check pydantic_ai_slim/pydantic_ai/ui/vercel_ai/_adapter.py pydantic_ai_slim/pydantic_ai/ui/ag_ui/_adapter.py tests/test_vercel_ai.py tests/test_ag_ui.py
All checks passed!

uv run ruff format --check pydantic_ai_slim/pydantic_ai/ui/vercel_ai/_adapter.py pydantic_ai_slim/pydantic_ai/ui/ag_ui/_adapter.py tests/test_vercel_ai.py tests/test_ag_ui.py
4 files already formatted
```

Initial upstream state, 2026-05-27 01:49 -03:

- PR open, ready, mergeable.
- CI started; most jobs pending.
- Passed immediately: Guard, Size Label, prompt fetch.
- `chatgpt-codex-connector` posted usage limit notice; no code action required.

Final CI check, 2026-05-27 02:00 -03:

- Fresh CI for commit `fec6e534` is green.
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

Watch CI and maintainer / bot review.

## Dashboard heartbeat - 2026-05-28 01:15 UTC

- PR `#5680` remains open, non-draft, `MERGEABLE/CLEAN`.
- Visible lint, mypy, docs, PR Guard, test matrix, examples, coverage, check, harness compat, UI security prompt fetch, and smokeshow contexts are green.
- No maintainer review yet; Codex review quota warning remains non-actionable.
- Same-file race risk: open PR `#5605` also touches `pydantic_ai_slim/pydantic_ai/ui/ag_ui/_adapter.py`, but it is not an exact duplicate for TextContent metadata round-trip.
- Upstream action count: `0`.
- Next action: wait maintainer/bot review; no new UI adapter work until review/merge ordering clears.
