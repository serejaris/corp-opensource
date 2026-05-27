# Pydantic AI TextContent Metadata Round-trip Watch

## Upstream

- Issue: https://github.com/pydantic/pydantic-ai/issues/5679
- Internal issue: https://github.com/serejaris/corp-opensource/issues/16
- Repo: `pydantic/pydantic-ai`
- Status: open bug, labelled `bug`, `message history`, `area:ui-adapters`, `pydanty:bug`
- Internal priority: candidate after `pydantic-ai#5678` reaches final CI state.

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

`GO after #5678`: strongest next pydantic candidate because it has exact code paths, no open duplicate PR found, and a narrow testable patch surface.
