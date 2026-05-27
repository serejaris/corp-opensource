# FastMCP / OpenAI Agents / DeepAgents scope watch, 2026-05-27

Tracker: [#73](https://github.com/serejaris/corp-opensource/issues/73)
Umbrella: [#52](https://github.com/serejaris/corp-opensource/issues/52)
Decision time: 2026-05-27 22:19 UTC
Final `next_status`: `WATCH`

## Scope

Bounded agent/MCP framework slice for popular Python AI-native runtimes.

| Repo | Fit | Live gate |
|---|---|---|
| `PrefectHQ/fastmcp` | MCP server/client framework with auth, HTTP/SSE transport, proxying, resource/link transforms and server lifecycle surfaces. | Apache-2.0; default `main`; `25354` stars / `2035` forks; `218` open issues / `15` open PRs; latest release `v3.3.1` published `2026-05-15T15:49:51Z`; pushed `2026-05-27T14:59:13Z`. |
| `openai/openai-agents-python` | Official Python agent SDK: tools, MCP, sessions, hooks, realtime, sandboxes and tracing. | MIT; default `main`; `26705` stars / `4111` forks; `45` open issues / `69` open PRs; latest release `v0.17.4` published `2026-05-26T08:54:24Z`; pushed `2026-05-26T08:53:37Z`. |
| `langchain-ai/deepagents` | Deep-agent harness with subagents, memory/filesystem middleware, CLI/code/evals and local testable agent runtime contracts. | MIT; default `main`; `23435` stars / `3314` forks; `132` open issues / `47` open PRs; latest release `deepagents==0.6.4` published `2026-05-26T18:54:00Z`; pushed `2026-05-27T21:04:50Z`. |

Required skills note: local skills `open-source-bug-scouting` / `open-source-pr-workflow` are not installed as callable skills in this session, so this cycle used the documented fallback: parent GitHub live gates plus 6-subagent scouting. No upstream comment/PR was attempted.

## Parent live gates

### PrefectHQ/fastmcp

- Community profile: health `87`, Apache-2.0, CONTRIBUTING, Code of Conduct, issue template, PR template and README are present.
- Test/repro gate: Python `>=3.10`; `uv` workspace; tests set `FASTMCP_TEST_MODE=1`, include `pytest-httpx`, `psutil`, `pytest-timeout`; conformance tests may need Node/npx.
- Process gate: FastMCP docs explicitly say an open issue is not an invitation for PR; PRs should be assigned or self-evident. Maintainer warned in `#4246` against unassigned PRs.
- Active duplicate/process clusters:
  - `#4246` OIDCProxy per-request `AsyncOAuth2Client` / httpx pool leak is the strongest lane, but maintainer PR `#4248` is active and discussion says close-only may not fully solve SNAT exhaustion versus shared-client reuse.
  - `#4205` OAuth discovery / Claude Desktop bootstrap is covered by `#4206`.
  - `#4161` proxy notification forwarding is covered by `#4191`.
  - `#4192` orphan `sse_starlette.EventSourceResponse` tasks is high-value but needs repeated-session evidence, likely Windows comparison, and may involve upstream MCP SDK behavior.
  - `#4143` event store eviction crash remains bug-shaped but was already tracked in [#55](https://github.com/serejaris/corp-opensource/issues/55) as assignment-first after MRE.

### openai/openai-agents-python

- Community profile: health `75`, MIT, Code of Conduct, issue template, PR template and README are present.
- Test/repro gate: Python `>=3.10`; `uv` workspace; pure SDK/session/hook bugs can use fake model/MCP fixtures; realtime/sandbox/provider lanes need broader runner or secret-safe setup.
- Active duplicate/process clusters:
  - `#3512` `RunHooksBase.on_tool_end` structured result type error is covered by open PR `#3513`; our evidence comment already exists.
  - `#3507` `types-requests` dependency cleanup is covered by `#3509`.
  - `#3348/#3346` session write/delete issues are covered by `#3508/#3498/#3478` and earlier duplicate PRs.
  - `#3477` MCP `_meta` is feature/design-heavy and has maintainer direction via `#3486` plus draft `#3480`.
  - Sandbox provider lanes are crowded: `#3468 -> #3469`, plus `#3500/#3502/#3504/#3482`.
- Process gate: small bugfix PRs with tests can be acceptable, but current narrow bug lanes are saturated. Only post-merge gap scan.

### langchain-ai/deepagents

- Community profile: health `100`, MIT, CONTRIBUTING, Code of Conduct, issue template, PR template and README are present.
- Test/repro gate: package `deepagents` `0.6.4`, Python `>=3.11,<4.0`; test dependencies include pytest/xdist/socket; pytest config disables benchmark by default and can support network-disabled local fixtures.
- Process gate: LangChain contribution guidance is issue/discussion-first with maintainer-approved solution for nontrivial PRs; labels split `internal` and `external`, and release/open-swe flow is active.
- Active duplicate/process clusters:
  - `#3639` MemoryMiddleware `add_cache_control=True` marking volatile memory is the best fresh lead; no direct covering PR found in the open PR list, but it is fresh and near release PR `#3602`.
  - `#3634` parent metadata propagation is covered by draft PR `#3635`.
  - `#3573` missing path errors is covered by PR `#3574`.
  - `#3587` Qwen/OpenAI-compatible empty tool-call id has our prior analysis comment and is `waiting-on-author`; maintainers suspect upstream LiteLLM/provider issue.
  - `#3631/#3583` loop guardrail work is internal/feature-heavy.

## 6-subagent synthesis

| Role | Result |
|---|---|
| Repo fit/activity | Ranking: OpenAI Agents strongest strategic SDK fit, FastMCP best pragmatic MCP/server contribution source, DeepAgents strong harness fit but internal/release-heavy. Recommended `WATCH`. |
| Issue quality/actionability | FastMCP `#4246` and DeepAgents `#3639` are the best future leads; OpenAI Agents current narrow lanes are covered. Recommended `WATCH`. |
| Duplicate/PR risk | OpenAI Agents and DeepAgents current lanes are mostly covered by active PRs; FastMCP has smaller PR queue but maintainer-owned auth/http PRs. Recommended `WATCH`. |
| Process/community gates | FastMCP is explicit assignment-first, DeepAgents requires maintainer-approved solution, OpenAI Agents is easier for small tested bugs but current lanes are occupied. Recommended `WATCH`. |
| Runner/repro feasibility | FastMCP has the best secret-free runner surface; DeepAgents has good local unit surface; OpenAI Agents is testable for core SDK but many lanes broaden to realtime/sandbox/provider. Recommended `WATCH`. |
| Final synthesis | No upstream action now. Trigger is FastMCP `#4248` gap, DeepAgents `#3639` current-main repro, or OpenAI Agents post-merge gap. Recommended `WATCH`. |

## Follow-up lanes

### PrefectHQ/fastmcp

- Watch `#4246/#4248`; promote only if `#4248` lands incomplete for SNAT/shared-client reuse and we have measured current-main evidence.
- Keep `#4192` as runner lane: repeated HTTP/SSE sessions, task/process/socket counts, Windows comparison if possible.
- Do not open unassigned PRs; comment only with new MRE/evidence.

### openai/openai-agents-python

- Watch `#3512/#3513`, `#3348/#3508/#3498`, `#3477/#3486/#3480`.
- Promote only a fresh pure-Python SDK regression with no linked PR, fake-model/MCP fixture, and targeted test.
- Avoid realtime/sandbox/provider work without secret-safe runner plan and maintainer direction.

### langchain-ai/deepagents

- Promote `#3639` only after reproducing on `0.6.x` and current `main`, identifying cache-control contract, and verifying no PR has appeared.
- Keep `#3587` watch-only until reporter provides raw provider/LiteLLM payload or maintainer says DeepAgents should accept empty ids.
- Avoid internal-labeled lanes and release automation PR clusters.

## 3-role critique

No upstream comment/PR is being sent, so this is an internal critique:

- Factology/duplicates: do not claim FastMCP `#4246` is open for a PR while maintainer PR `#4248` is active. Do not claim DeepAgents `#3639` is duplicate-clean without a broader search/repro. Do not reopen OpenAI Agents lanes that are already covered by active PRs.
- Process gates: FastMCP assignment-first rule is explicit; DeepAgents nontrivial PRs need maintainer-approved direction; OpenAI Agents requires careful linked issue/test plan but is still not open for duplicate lanes.
- Actionability: exactly one status is selected: `WATCH`. Unblock event is FastMCP `#4248` post-merge gap, DeepAgents `#3639` runner/local repro, or a fresh OpenAI Agents pure-SDK bug.

## Runner

Runner was not used in this cycle. Reason: repo/issue/PR scouting only; no issue reached `CANDIDATE` or status-changing `COMMENT-FIRST`. Dedicated `corp-opensource-runner` is useful for FastMCP repeated HTTP/SSE evidence and targeted DeepAgents/OpenAI Agents unit repros. CT `216` remains Hermes-specific and was not used.

## Decision

Keep all three repos in scope, but do not open upstream comments or PRs now.

- `PrefectHQ/fastmcp`: best immediate MCP/server follow-up, blocked by `#4248` and assignment-first gate.
- `langchain-ai/deepagents`: best fresh secret-free regression lead is `#3639`, blocked on repro and duplicate gate.
- `openai/openai-agents-python`: strategic watch, but current obvious lanes are PR-covered.
