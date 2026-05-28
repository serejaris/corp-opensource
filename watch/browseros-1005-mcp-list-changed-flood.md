# BrowserOS #1005 MCP list_changed flood, 2026-05-27

Tracker: [#81](https://github.com/serejaris/corp-opensource/issues/81)
Upstream: [browseros-ai/BrowserOS#1005](https://github.com/browseros-ai/BrowserOS/issues/1005)

## Scope

Issue-level follow-up from [cycle 30](ai-native-popular-repos-paperclip-scouting-2026-05-27-cycle-30-repo-universe.md) for the BrowserOS MCP server flood lane.

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this cycle used the documented fallback: 6 read-only scouting roles, parent live GitHub gates through `gh`, then 3-role critique.

No upstream PR/comment/ping was made.

## Parent live gates

Live state at `2026-05-27 23:56-23:59 UTC`:

- Repo `browseros-ai/BrowserOS` is active, default branch `main`, root `LICENSE` is AGPL-3.0, latest release `v0.44.0.1`, pushed `2026-05-27T23:56:38Z`.
- Upstream issue `#1005` is open, unassigned and unlabeled. It was created `2026-05-15T14:20:15Z` and updated by the reporter on `2026-05-26T14:07:16Z`.
- Reported symptom: built-in MCP server `browseros_mcp v0.0.79`, Streamable HTTP endpoint `http://127.0.0.1:9001/mcp`, emits `notifications/tools/list_changed` around 3/sec while idle.
- Reporter follow-up: each MCP tool call re-registers all ~60 tools, the tool list is byte-identical across repeated registrations, and re-registration appears to trigger the notification.
- Exact PR search by `1005`, `list_changed`, `notifications/tools/list_changed`, `tools/list_changed`, and the issue title found no covering PR.
- Adjacent MCP context exists: `#944`, `#864`, `#1031`, `#948`, `#1065`. None is an exact duplicate of the repeated `list_changed` notification flood.
- Contribution/process gate: root `CONTRIBUTING.md` has a lightweight agent-development path for `packages/browseros-agent`, but first PR requires CLA acknowledgement. Full Chromium build is out of scope for this lane.

## Source gate

Source-path hypothesis on upstream `main`:

- `packages/browseros-agent/apps/server/src/api/routes/mcp.ts`: `POST /mcp` constructs a fresh `createMcpServer(...)` and `StreamableHTTPTransport`.
- `packages/browseros-agent/apps/server/src/api/services/mcp/mcp-server.ts`: `createMcpServer()` calls `registerTools(...)` and optionally `registerKlavisTools(...)`.
- `packages/browseros-agent/apps/server/src/api/services/mcp/register-mcp.ts`: `registerTools()` loops through `registry.all()`, calls `mcpServer.registerTool(...)`, and logs `Registered ${registry.names().length} tools...`.
- `packages/browseros-agent/apps/server/src/tools/registry.ts`: static tool registry; reporter saw the same tool list across repeated registrations.

Hypothesis: the server recreates / re-registers an unchanged MCP tool registry during request or tool-call handling, and the MCP server layer broadcasts `notifications/tools/list_changed` for identical tools. This is plausible but not yet runner-proven.

GitHub code search hit a rate-limit during source scan; parent fell back to repository contents and path reads instead of relying on search results.

## 6-subagent synthesis

- Factology: `#1005` is a live, detailed bug report with no labels, no assignee, no exact PR, and a narrow MCP/server symptom. Recommended `CANDIDATE`.
- Duplicate role: exact coverage was not found. Nearby MCP PRs are adjacent only, so collision risk is medium, not blocking.
- Source/test role: the server-side code path is small enough for focused Bun/MCP integration coverage; likely test surface is under `packages/browseros-agent/apps/server/tests`.
- Process/license role: AGPL + CLA are manageable, but a PR needs runner proof and CLA awareness. Chromium-core build is not part of this lane.
- Repro/runner role: comment-first is possible only after adding a new fact; PR-ready requires secret-free runner reproduction.
- Synthesis role: promote `#1005` from repo-card lead to internal `CANDIDATE`, with upstream action count `0`.

## 3-role critique

- Factology/duplicates: `CANDIDATE` is justified. `COMMENT-FIRST` is weaker because the reporter already supplied logs, root-cause direction and impact.
- Process gates: internal tracker/watch update only. Runner proof is not required to record a candidate, but it is required before any status-changing upstream technical comment or PR.
- Actionability: `next_status: CANDIDATE`. Not `PR-READY` because repeated `registerTool` has not yet been proven as the broadcast source in the current server version, and no minimal patch boundary has been chosen.

## Decision

`next_status: CANDIDATE`

Upstream action count: `0`.

Reason: strong reporter evidence, active repo, exact PR duplicate not found, and a plausible narrow server-side path. The lane remains blocked on dedicated runner reproduction and a focused regression card before any upstream comment or PR.

## Regression card draft

Target package: `packages/browseros-agent/apps/server`

Likely command after repo setup:

```bash
cd packages/browseros-agent/apps/server
bun run test:integration
```

Focused test shape:

- Start or construct the MCP server with the stable static registry.
- Connect with Streamable HTTP MCP client or a server-side transport fixture.
- Trigger repeated request/tool-call paths that currently cause `Registered ... tools` re-registration.
- Assert unchanged registry hash/name/schema does not emit repeated `notifications/tools/list_changed`.
- Assert a changed registry hash/schema emits exactly one `notifications/tools/list_changed`.

Fail-before is not yet proven. The first runner task is to turn the reporter's reproduction into a minimal test or harness observation.

## Runner note

Dedicated `corp-opensource-runner` was not used in this cycle because it is not confirmed as provisioned/available here. CT `216` remains Hermes-only and was not used.

## Promotion triggers

- `COMMENT-FIRST`: maintainer asks for direction, a fix-approach dispute appears, or we can add a short new fact from runner/source testing.
- `PR-READY`: runner-backed fail-before, green focused test, fresh duplicate PR search, CLA/process note, and 3-role critique.
- `NO-GO`: exact upstream PR appears, issue closes as fixed, or the notification is proven correct by MCP lifecycle semantics rather than a BrowserOS bug.

## Repo-universe heartbeat - 2026-05-28 01:45 UTC

- Live issue remains open and unassigned.
- Reporter's 2026-05-26 reproduction remains the strongest evidence: each MCP tool call re-registers an identical tool list and emits `notifications/tools/list_changed`.
- Targeted open PR search again found no exact cover for `#1005`.
- 3-role critique kept `next_status: CANDIDATE`; upstream action count `0`.
- No runner repro was run; promotion still requires current-main fail-before evidence in `corp-opensource-runner`.
