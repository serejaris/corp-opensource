# LangGraph #7688: TIME_WAIT falsely blocks `langgraph dev`

Date: 2026-05-27
Upstream: https://github.com/langchain-ai/langgraph/issues/7688
Internal issue: https://github.com/serejaris/corp-opensource/issues/48

## Почему в scope

`langgraph dev` falsely reports `Port 2024 already in use` when the port is only in `TIME_WAIT`.

This is an AI-native dev/runtime harness bug: local agent/runtime servers become flaky even when the actual listener is gone.

## Current live state

- Upstream issue is open.
- Labels: `bug`, `external`.
- Assignees: none.
- Comments include one external offer to work if triaged and one `+1` user-pain comment.
- Six-agent duplicate pass did not find an exact competing PR, but GitHub Search API was rate-limited during broad search. Re-run duplicate gate before any code or upstream comment.

## Regression card draft

- Contract: `langgraph dev` should treat a `TIME_WAIT` connection as reusable/non-listening and should not report `Port 2024 already in use` when no process is accepting connections.
- Repro idea: socket-level fixture around the same port-check helper used by CLI, using bind/close/rebind or a subprocess that leaves `TIME_WAIT`.
- Likely command: targeted pytest around CLI/server port resolution once local test surface is identified.
- Risk: OS socket semantics may differ; prefer Linux runner (`corp-opensource-runner`) when available.

## Local repo reconnaissance

Checkout:

`~/Documents/GitHub/langgraph-7688`

Findings:

- `langgraph dev` is implemented in `libs/cli/langgraph_cli/cli.py`.
- The command does not appear to perform its own port-availability check.
- It imports `run_server` from `langgraph_api.cli` and passes `host` / `port` through.
- `langgraph-api` is an optional dependency of `langgraph-cli[inmem]`, not source code in this checkout:
  - `libs/cli/pyproject.toml`: `langgraph-api>=0.5.35,<0.9.0`.
- A broad local `rg` did not find an obvious `TIME_WAIT` / `already in use` / socket listener helper inside `libs/cli`.

Current classification: `WATCH / needs-repro`.

This may be a `langgraph-api` behavior rather than a patchable bug in the open `langgraph` repo. A PR is only appropriate if a preflight fix belongs in `langgraph-cli` or if the relevant `langgraph-api` source/test surface is available.

## Next action

1. Inspect installed `langgraph-api` package source locally if available after `uv sync --extra inmem`.
2. If the port check lives in `langgraph-api`, do not open a PR in `langgraph` unless maintainers say that package source is contribution-available here.
3. If a CLI-side preflight is viable, prove pre-fix failure with the smallest targeted test.
4. If no clean pre-fix failure, keep `WATCH / needs-repro` and do not comment upstream.
