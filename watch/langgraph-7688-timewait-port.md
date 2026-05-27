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

## Next action

1. Read LangGraph contribution rules and exact test harness.
2. Clone/check current `main`.
3. Find port-resolution code path.
4. Prove pre-fix failure with smallest targeted test.
5. If no clean pre-fix failure, keep `WATCH / needs-repro` and do not comment upstream.
