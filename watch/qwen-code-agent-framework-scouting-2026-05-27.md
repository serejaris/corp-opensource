# Qwen Code / Agent Framework scouting, 2026-05-27

Tracker: [#68](https://github.com/serejaris/corp-opensource/issues/68)
Umbrella: [#52](https://github.com/serejaris/corp-opensource/issues/52)
Decision time: 2026-05-27 21:22 UTC
Final `next_status`: `WATCH`

## Scope

Bounded two-repo scouting slice for AI-native / Paperclip-like contribution strategy.

| Repo | Fit | Live gate |
|---|---|---|
| `QwenLM/qwen-code` | Terminal coding agent: CLI UX, session state, tool execution, provider/runtime edge cases. | Apache-2.0; default `main`; created `2025-06-26`; `24711` stars / `2418` forks; `650` open issues / `153` open PRs; latest release `v0.16.2` published `2026-05-27T09:31:00Z`; pushed `2026-05-27T18:34:10Z`. |
| `microsoft/agent-framework` | Python/.NET multi-agent workflow framework: orchestration, workflow state, function/tool calling, AG-UI/Durable Agents. | MIT; default `main`; created `2025-04-28`; `10793` stars / `1788` forks; `673` open issues / `266` open PRs; latest release `python-1.6.0` published `2026-05-22T02:14:07Z`; pushed `2026-05-27T21:12:49Z`. |

Required skills note: local skill names `open-source-bug-scouting` / `open-source-pr-workflow` are not installed as callable skills in this session, so the cycle used the documented fallback: parent GitHub live gates plus 6-subagent scouting. No upstream comment/PR was attempted.

## Parent live gates

### QwenLM/qwen-code

- Community profile: health `75`, has README, Apache-2.0 license, CONTRIBUTING and PR template.
- Contribution gate: existing issue first, small focused PR, `npm run preflight`, docs update for user-facing behavior, screenshot/video demo for user-facing fixes.
- Test/repro gate: Node.js `>=22`; scripts include `test`, `test:ci`, integration test variants, `lint`, `typecheck`, `preflight`.
- High duplicate pressure:
  - `#4450` `qwen --list-extensions` no-op already has open PR `#4456`.
  - `#4466` `${VAR}` headers from `.env` already has open PR `#4474`.
  - `#4363/#4364` oversized history/stdout lanes overlap open PRs `#4531/#4524`.
  - DashScope thinking/output language lanes overlap `#4505/#4519`.
  - `#4579` compressed-turn resume bug is assigned.

### microsoft/agent-framework

- Community profile: health `87`, has README, MIT license, CONTRIBUTING, PR template, CODE_OF_CONDUCT, SECURITY and SUPPORT docs.
- Contribution gate: search before filing, minimal repro, state intent before taking work, tests required, avoid large/API changes without issue discussion.
- Test/repro gate:
  - Python: `uv` / `poe` flow; unit tests can be secret-free, integration may require endpoints.
  - .NET: `dotnet build` / targeted tests; API compatibility matters for packages.
- High duplicate pressure:
  - `#6027/#6028` Foundry headers/stream-wrapper issues overlap open PRs `#6040/#6029`.
  - `.NET` inline skill args overlap `#6047/#6033`.
  - AG-UI text chunk behavior overlaps `#6048/#6011`.
  - approval response semantics overlap `#6036`.
  - hosted MCP reasoning overlaps `#6070/#6110`.

## 6-subagent synthesis

| Role | Result |
|---|---|
| Repo fit/activity | Both repos are strategically relevant; Qwen is the more tactical terminal-agent lane, Agent Framework is a strategic orchestration/reference lane. Recommended `WATCH`. |
| Issue quality/actionability | Qwen best later lanes: `#4450`, `#4466`; Agent Framework best later lanes: `#6120`, `#6006`. No upstream action because duplicate gates and runner repro are missing. Recommended `WATCH`. |
| Duplicate/PR risk | Strongest issue lanes are already covered or adjacent to active PRs in both repos. Recommended `WATCH`. |
| Process/community gates | Direct PR is not process-safe now. Only issue/comment-first after duplicate search and concrete repro. |
| Runner/repro feasibility | Dedicated `corp-opensource-runner` needed before status-changing upstream evidence; CT `216` is Hermes-specific and not appropriate. Recommended `WATCH`. |
| Actionability synthesis | Record bounded-cycle result as `WATCH`; no upstream comment/PR; next gate is one minimal runner repro plus fresh duplicate scan. |

## Candidate lanes to watch

### QwenLM/qwen-code

- `#4450` remains a useful CLI-extension reference, but open PR `#4456` blocks duplicate work.
- `#4466` remains useful for config/env interpolation, but open PR `#4474` blocks duplicate work and the area is credential/security-adjacent.
- `#4568` submodule file completion may be a future narrow lane after triage and PR search.
- `#4579` session rewind/compressed-turn looks high quality, but assignment blocks our action.

Promotion gate: one exact issue, fresh PR search by issue number and subsystem terms, Node `>=22` runner checkout, targeted failing repro/test, then `npm run test` or package-level equivalent; `npm run preflight` plan required before PR.

### microsoft/agent-framework

- `#6120` `.NET` history-provider behavior is the best unassigned watch lane after duplicate search.
- `#6006` DevUI deserialization is another possible exact parser lane after duplicate search.
- `#6061/#6063` DurableAIAgentProxy / AG-UI remains valuable but likely design/runtime-heavy and should be comment-first only after MRE.
- `#6074` and `#6027/#6028` are strong but assigned/covered and should stay watch-only.

Promotion gate: choose one subsystem (`python` or `.NET`), fresh issue+PR duplicate search, secret-free runner repro, exact command evidence and targeted regression test before any upstream comment.

## 3-subagent critique

No upstream comment/PR is being sent, so the critique is recorded as an internal gate rather than an action approval:

- Factology/duplicates: do not overclaim any candidate as uncovered; multiple strongest lanes already have open PRs or assignment.
- Process gates: both repos require issue-first/minimal-repro discipline; direct PR from this cycle would violate the local `PR-READY` bar.
- Actionability: exactly one status is selected: `WATCH`. Unblock event is runner-backed repro plus fresh duplicate search for one exact issue.

## Runner

Runner was not used in this cycle. Reason: this was a repo/issue/PR scouting slice only, and no candidate reached `CANDIDATE` or `COMMENT-FIRST` with a specific test card. Dedicated `corp-opensource-runner` is still required before any status-changing upstream evidence. CT `216` remains Hermes-specific and was not used.

## Decision

Keep both repos in scope, but do not open upstream comments or PRs now.

- `QwenLM/qwen-code`: keep Tier `2/3` verify-first with exact CLI/config/session lanes only.
- `microsoft/agent-framework`: keep Tier `2/3` watch with exact Python/.NET subsystem gates only.
- Next bounded cycle should start with one issue, not a repo-wide sweep.
