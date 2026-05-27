# AI-native frameworks scouting: cycle 17

Date: 2026-05-27
Mode: 6-subagent scouting + targeted upstream follow-up.

## Constraint

GitHub Search API hit rate limit during broad issue search. The cycle used direct `gh issue view`, `gh pr view`, issue timeline/open PR checks, and subagent live reads where possible.

## Six-agent outputs

| Role | Key result |
|---|---|
| Repo-fit | Strongest active repos remain `pydantic-ai`, `trycua/cua`, `browser-use`, `OpenHands`, `cline`, `modelcontextprotocol/python-sdk`, `playwright-mcp`, `goose`; `opencode-ai/opencode` is archived and not a target. |
| Bug-signal | Best signals: `pydantic-ai#5688`, `modelcontextprotocol/python-sdk#2678`, `browser-use#4846`, `trycua#1724`, `openai-agents-python#3512`, `cline#10737`, `langgraph#7880/#7688`. |
| Repro-path | Best cheap repro remains `pydantic-ai#5688`; next viable repro-first candidate is `langgraph#7688` if socket/TIME_WAIT can be proved locally or on runner. |
| Patchability | `pydantic-ai#5688` is XS but gated; `modelcontextprotocol/python-sdk#1664` has competing PR; `google/adk-js#377` is high-value but lifecycle-design sensitive. |
| Duplicate-race | No exact PR found for `pydantic-ai#5688` or `google/adk-python#5864`; both are comment-first/gated. `typescript-sdk#2155` is server-side duplicate-covered by `#1925/#1926`; only unresolved gap is reporter validation/client fail-fast. |
| PR-readiness | `pydantic-ai#5688` and `google/adk-python#5864` are technically PR-ready but should wait for maintainer/reporter confirmation. `modelcontextprotocol/python-sdk` and LangGraph have assignment/approval risk. |

## Live follow-up checked

- `pydantic-ai#5688`: open, no assignee, no exact PR found. Local regression-first branch exists. Continue waiting for assignment/confirmation or pydanty PR.
- `google/adk-python#5864`: open, assigned to `surajksharma07`, labels `core`, `request clarification`; maintainer asked reporter to validate and run full test pass. Continue waiting.
- `pydantic-ai#5680`: open, mergeable, CI/coverage green; Codex review quota comment only. Watch.
- `cline#11087`: open, mergeable, Quality Checks + Ubuntu/Windows SDK tests green; follow-up review gap already addressed. Watch.
- `opencode#29565`: open, mergeable, duplicate/compliance/standards checks green. Watch.
- `typescript-sdk#2155`: we already posted duplicate-triage; server-side SSE escaping covered by `#1925/#1926`, both open, mergeable, green. No new PR.

## New candidate promoted

Internal issue: https://github.com/serejaris/corp-opensource/issues/48

Candidate: `langchain-ai/langgraph#7688`

Why:

- AI-native dev/runtime harness bug: `langgraph dev` falsely reports `Port 2024 already in use` when the port is only in `TIME_WAIT`.
- Upstream issue is open, labels `bug`, `external`, no assignee.
- User pain signal: one `+1` comment.
- Six-agent pass did not find an exact competing PR.

Regression draft:

- Contract: `langgraph dev` should not treat `TIME_WAIT` as an active listener.
- Test surface: find CLI/server port-resolution helper first.
- Repro idea: socket-level fixture using bind/close/rebind or subprocess leaving `TIME_WAIT`.
- Risk: OS socket semantics; prefer Linux runner when available.

## Decision

No upstream PR or comment this cycle.

Follow-up on `langgraph#7688` after local clone:

- `langgraph dev` in `libs/cli/langgraph_cli/cli.py` imports `run_server` from `langgraph_api.cli` and passes `host` / `port` through.
- No obvious port-availability helper was found in the open `libs/cli` source.
- `langgraph-api` is an optional dependency (`langgraph-cli[inmem]`), not source code in this checkout.
- Therefore `langgraph#7688` is downgraded from `candidate` to `WATCH / needs-repro` until we can prove the bug belongs in open `langgraph-cli` or inspect the relevant `langgraph-api` test surface.

Next concrete work:

1. For `langgraph#7688`, inspect installed `langgraph-api` package source after `uv sync --extra inmem`, if practical.
2. If no open test surface exists, do not upstream-comment or PR.
3. Keep `pydantic-ai#5688` and `google/adk-python#5864` hot but do not race maintainers.
