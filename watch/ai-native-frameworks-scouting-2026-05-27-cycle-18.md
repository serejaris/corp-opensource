# AI-native frameworks scouting: cycle 18

Date: 2026-05-27
Mode: 6-role subagent scouting + live duplicate/follow-up checks.
Internal tracker: https://github.com/serejaris/corp-opensource/issues/49

## Outcome

Safe new ready PR: none.

Useful action completed: closed `e2b-dev/E2B#1354` as superseded by maintainer/superset `#1355`.

## Six-role synthesis

| Role | Finding |
|---|---|
| Repo-fit | Best live repos by fit: `OpenHands/software-agent-sdk`, `trycua/cua`, `modelcontextprotocol/typescript-sdk`, `pydantic-ai`, `cline`, `google/adk-python`, `goose`. `opencode` remains active but noisy; `browser-use` currently has weaker maintainer signal. |
| Bug-signal | Fresh signals worth triage: `goose#9446`, `opencode#29498`, `opencode#29518`, `opencode#29534`, `langgraph#7915`, `playwright-mcp#1635`, `trycua#1724`. |
| Repro-path | Best no-secret regression paths: `pydantic-ai#5688`, `google/adk-python#5864`, `OpenHands SDK#3317`. `langgraph#7688` is reproducible but should use Linux/runner for stronger TCP evidence. |
| Patchability | Cleanest small patches remain `pydantic-ai#5688` and `google/adk-python#5864`, both gated by upstream process. `MCP Python#2687` became duplicate-covered by `#2701`. |
| Duplicate-race | `E2B#1354` is superseded by maintainer `#1355`. `MCP Python#2687` is covered by `#2701`. Browser-use and several MCP TS/Python candidates remain duplicate-covered. |
| PR-readiness | Ready lanes exist in `OpenHands/software-agent-sdk`, `trycua/cua`, `goose`, and tiny MCP TS bugs; `pydantic-ai`, `ADK`, `MCP Python`, `LangGraph`, and Cline nontrivial changes are comment-first or assignment-gated. |

## Live follow-up

| Upstream | Current state | Decision |
|---|---|---|
| `e2b-dev/E2B#1354` | Closed as superseded. Our PR only covered JS and was CLA-blocked. | Track `#1355`; no further work on `#1354`. |
| `e2b-dev/E2B#1355` | Open maintainer PR; covers JS + Python SDK multi-source `COPY`/`ADD`; CLA and Vercel green; SDK checks still running in tracked state. | Watch. |
| `modelcontextprotocol/python-sdk#2701` | Open, mergeable; covers `#2687`; most CI green, one Windows locked job failed while all-green reported success. | Watch existing PR; no duplicate. |
| `pydantic-ai#5688` | Open; pydanty confirmed root cause; our availability comment remains latest external action. | Wait assignment/confirmation. |
| `google/adk-python#5864` | Open; assigned, `request clarification`; no reply after our offer. | Wait maintainer/reporter direction. |
| `langgraph#7688` | Open; our repro comment has one thumbs-up; no maintainer package-surface direction. | Watch. |
| `OpenHands/software-agent-sdk#3394` | Open, mergeable; maintainer wants eval; earlier bot `CHANGES_REQUESTED` remains. | Wait eval/human maintainer. |

## Fresh candidate backlog

| Candidate | Why it matters | Current action |
|---|---|---|
| `aaif-goose/goose#9446` | `chatgpt_codex` stream decode fails when `response.completed` omits `output`. Directly relevant to Codex/Hermes stream robustness. | Create regression card next: fixture stream event with completed/no-output, compare decoder behavior. |
| `anomalyco/opencode#29498` | Windows subagent path mapping can point edits to temp files instead of workspace files. | Needs Windows/path repro before PR. |
| `anomalyco/opencode#29518` | Strict function-calling providers reject tool schema with missing `description`, causing retry loop. | Check schema generator and duplicate PRs before repro. |
| `anomalyco/opencode#29534` | CJK tool-call params persist as replacement chars in SQLite. | DB roundtrip regression candidate. |
| `langchain-ai/langgraph#7915` | SSE decoder may join repeated `data:` lines incorrectly. | Small unit-test candidate, but check issue timeline/assignment first. |
| `OpenHands/software-agent-sdk#3317` | Kimi model token limit inference may send unsupported value. | No-secret constructor/config regression possible; check duplicate/maintainer signal first. |
| `modelcontextprotocol/typescript-sdk#2155` | Server-side SSE escaping already covered by `#1925/#1926`; remaining possible gap is client fail-fast / JSON-response validation. | Watch/validate only; no server-side duplicate PR. |

## Decision

Do not open a fresh PR until one of these is true:

1. `pydantic-ai#5688` or `google/adk-python#5864` gets maintainer/assignment confirmation.
2. A fresh candidate gets a public contract regression and passes duplicate gate.
3. Maintainers confirm `langgraph#7688` should be patched in a public package surface.

Next concrete pass should start with `goose#9446` because it is Codex/Hermes-adjacent, fresh, unassigned, and likely fixture-testable without secrets.

## Goose #9446 patch prep

Local regression-first patch prepared after this cycle, then opened upstream PR:

- Watch note: [goose-9446-response-completed-missing-output.md](goose-9446-response-completed-missing-output.md)
- Branch: `/Users/ris/Documents/GitHub/goose` -> `codex/9446-openai-response-completed-default-output`
- Commit: `f4d5dd6b2 fix(providers): tolerate missing Responses output`
- PR: https://github.com/aaif-goose/goose/pull/9447
- Pre-fix fail: `Failed to parse Responses stream event: missing field output`
- Post-fix: targeted regression passed; all `providers::formats::openai_responses::tests` passed; `cargo fmt --check` passed.

Initial PR state: open, ready, mergeable; checks pending/skipped in tracked state.
