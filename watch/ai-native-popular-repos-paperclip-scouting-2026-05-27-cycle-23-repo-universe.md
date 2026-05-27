# AI-native popular repo universe, cycle 23, 2026-05-27

Live state: parent `gh repo view` pass на 2026-05-27 17:20 UTC.
Tracker: [serejaris/corp-opensource#52](https://github.com/serejaris/corp-opensource/issues/52).

Upstream actions в этом блоке: 0 PR, 0 comments.

Process gate: repo-level scouting only. No bug-level candidate or upstream PR target selected in this cycle.

Order confirmed:

1. 6-subagent scouting synthesis completed and recorded.
2. Parent live gates checked: internal tracker issue `#52` state/labels, repo state, canonical owner, activity, queue size, contribution docs where relevant, repo-scope/regression note. Bug-level linked PRs/timeline are N/A because no issue candidate was selected; repo-level PR search is represented by current open PR queue and duplicate-risk mode.
3. 3-subagent critique completed: factology/duplicates, process gates, actionability.
4. Tracker/watch updates applied after gates.

Runner/repro gate: N/A for this cycle because no concrete bug/PR target was promoted.

Allowed next actions are repo-level only: `promote`, `demote`, `watch`, `create repo-card`, `update scope`, `close stale tracker`. No `PR-READY` status.

## Scope

Этот блок не выбирал отдельный баг. Цель: расширить и упорядочить repo universe для Paperclip-like / AI-native strategy.

`open-source-bug-scouting` и `open-source-pr-workflow` skills в текущей среде недоступны, поэтому блок выполнен как documented fallback поверх live GitHub gates и subagent scouting. Subagent tools доступны и использованы: 6 ролей для discovery/fit, затем 3 роли critique.

Runner/repro gate: not applicable для этого блока, потому что выбран repo-level scope update, без candidate bug и без upstream PR/comment.

## Cycle 1: Research / Discovery

Слои поиска:

- terminal coding agents / IDE agents: `openai/codex`, `cline/cline`, `OpenHands/OpenHands`, `anomalyco/opencode`, `google-gemini/gemini-cli`, `continuedev/continue`, `Aider-AI/aider`;
- MCP protocol/runtime/tools: `modelcontextprotocol/python-sdk`, `modelcontextprotocol/typescript-sdk`, `microsoft/playwright-mcp`, `PrefectHQ/fastmcp`, Docker MCP repos;
- browser / computer-use: `browser-use/browser-use`, `trycua/cua`, `browserbase/stagehand`, `steel-dev/steel-browser`, `seleniumbase/SeleniumBase`;
- agent frameworks / skills / eval: `pydantic/pydantic-ai`, `langchain-ai/langgraph`, `langchain-ai/deepagents`, `crewAIInc/crewAI`, `google/adk-python`, `microsoft/autogen`;
- sandbox / session orchestration: `e2b-dev/E2B`, `daytonaio/daytona`, Morph public SDKs, Modal examples;
- cross-repo health: tests/docs, maintainer rhythm, queue noise, duplicate risk.

## Parent Live Gates

| Repo | Stars | Pushed | Release | Open issues | Open PRs | Decision |
|---|---:|---|---|---:|---:|---|
| `anomalyco/opencode` | 166083 | 2026-05-27 | `v1.15.11` | 5265 | 940 | `WATCH`, strategic Tier 1, extreme churn |
| `google-gemini/gemini-cli` | 104656 | 2026-05-27 | `v0.43.0` | 1198 | 302 | `WATCH`, strategic Tier 1, comment-first only |
| `browser-use/browser-use` | 95853 | 2026-05-26 | `0.12.9` | 66 | 177 | Tier 2 watch/comment-first |
| `openai/codex` | 86281 | 2026-05-27 | `rust-v0.134.0` | 4944 | 301 | `WATCH`, strategic Tier 1, no blind PR hunting |
| `OpenHands/OpenHands` | 75049 | 2026-05-27 | `1.7.0` | 164 | 202 | promote active scouting |
| `daytonaio/daytona` | 72452 | 2026-05-27 | `v0.182.0` | 275 | 130 | strategic Tier 1, watch/comment-first |
| `cline/cline` | 62408 | 2026-05-27 | `v3.85.0` | 492 | 471 | promote active scouting, strict duplicate gate |
| `microsoft/playwright-mcp` | 33098 | 2026-05-23 | `v0.0.75` | 5 | 4 | promote precise protocol/browser regressions |
| `PrefectHQ/fastmcp` | 25344 | 2026-05-27 | `v3.3.1` | 218 | 15 | Tier 2 active watch |
| `langchain-ai/deepagents` | 23432 | 2026-05-27 | `deepagents==0.6.4` | 134 | 45 | promote active scouting |
| `modelcontextprotocol/python-sdk` | 23149 | 2026-05-27 | `v1.27.1` | 251 | 241 | Tier 2 comment-first |
| `browserbase/stagehand` | 22824 | 2026-05-27 | `stagehand-server-v3/v3.6.10` | 91 | 136 | Tier 2 watch |
| `pydantic/pydantic-ai` | 17338 | 2026-05-27 | `v1.103.0` | 384 | 179 | promote active scouting |
| `trycua/cua` | 17136 | 2026-05-27 | `cua-driver-v0.2.0` | 128 | 209 | promote domain scouting, repro-first |
| `modelcontextprotocol/typescript-sdk` | 12541 | 2026-05-27 | `v1.29.0` | 222 | 207 | Tier 2 comment-first |
| `e2b-dev/E2B` | 12375 | 2026-05-27 | `e2b@2.26.0` | 27 | 33 | promote active scouting |

Note: subagent issue/PR counts from GitHub Search sometimes included API secondary-rate partials. Parent table above uses `gh repo view` GraphQL counts and is the authoritative count for this note. Counts can drift minute by minute; values above were refreshed after critique for the high-churn repos.

## Cycle 2: Triage / Fit

### Tier 1A: Active Scouting Targets

| Repo | Paperclip layer | Why | Operating mode |
|---|---|---|---|
| `pydantic/pydantic-ai` | skills/capabilities, tools, providers, eval/observability | Strong tests/docs, many narrow provider/tool/MCP surfaces. | Active scouting for regression-first bugs; duplicate sweep before code. |
| `OpenHands/OpenHands` | control-plane, workspace agent, skills, sandbox | High Paperclip fit and healthier issue queue than the largest terminal agents. | Comment-first when roadmap ambiguity exists; PR only after maintainer/repro gate. |
| `e2b-dev/E2B` | sandbox/session orchestration | Low-noise queue and direct sandbox relevance. | Active scouting for SDK/session/docs bugs; avoid cloud-heavy fixes without runner. |
| `cline/cline` | IDE/coding-agent runtime, MCP, provider adapters | Strong user impact and deterministic extension/CLI surfaces. | Active scouting, but strict duplicate/Linear/maintainer gate. |
| `trycua/cua` | computer-use runtime, desktop drivers, benchmarks | Best computer-use fit. | Repro-first on native/runner env; no PR without current platform evidence. |
| `microsoft/playwright-mcp` | browser automation MCP runtime | Small backlog, canonical tool boundary, precise regression surface. | Comment-first or narrow PR after minimal fixture. |
| `langchain-ai/deepagents` | skills/capabilities harness | Smaller than LangGraph, direct subagent/filesystem/tooling relevance. | Active scouting for harness and tool-call regressions. |

### Tier 1B: Strategic Watch, Not Blind PR Hunting

| Repo | Why Tier 1 | Why watch-first |
|---|---|---|
| `openai/codex` | Direct terminal coding-agent reference. | 4943 open issues, 302 PRs, high duplicate race; external contribution lanes need very narrow gates. |
| `anomalyco/opencode` | Huge active terminal-agent ecosystem. | 5265 open issues, 939 PRs; current local repo already has multiple open PRs, avoid piling on. |
| `google-gemini/gemini-cli` | Official terminal CLI agent and MCP-aware workflow. | Corporate/high-churn repo; comment-first unless repro is crisp and duplicate gate is clean. |
| `daytonaio/daytona` | Secure dev env / runner control-plane, close to Paperclip worker/session needs. | High velocity and broad infra surface; take only sharply bounded runner/session bugs. |

### Tier 2: Watch / Comment-First

| Repo | Layer | Mode |
|---|---|---|
| `browser-use/browser-use` | browser-agent framework | Watch/comment-first; public queue is noisy despite strong relevance. |
| `browserbase/stagehand` | AI browser SDK | Watch; avoid broad SDK PRs without maintainer confirmation. |
| `PrefectHQ/fastmcp` | MCP framework/runtime | Active watch; good user-facing bugs, but less spec-canonical than official SDKs. |
| `modelcontextprotocol/python-sdk` | MCP protocol/runtime | Comment-first; spec churn and heavy PR queue. |
| `modelcontextprotocol/typescript-sdk` | MCP protocol/runtime | Comment-first; duplicate gate must be fresh. |
| `google/adk-python` | agent SDK/eval/deploy | Watch; Tier 1 only for precise regression/security/eval bugs. |
| `continuedev/continue` | IDE/CLI/CI agent workflow | Watch integration/config bugs; verify product-direction fit first. |
| `crewAIInc/crewAI` | multi-agent orchestration | Watch/comment-first because PR queue is saturated. |
| `langchain-ai/langgraph` | graph/runtime orchestration | Watch; crowded, fast-moving, only crisp regressions. |

Tier 2 split for the next cycle:

- `comment-first eligible`: `modelcontextprotocol/python-sdk`, `modelcontextprotocol/typescript-sdk`, `microsoft/playwright-mcp` only when there is an issue with incomplete diagnosis and a low duplicate signal.
- `watch`: `browser-use/browser-use`, `browserbase/stagehand`, `google/adk-python`, `continuedev/continue`, `crewAIInc/crewAI`, `langchain-ai/langgraph` until a concrete issue-level lane passes gates.
- `active watch`: `PrefectHQ/fastmcp` for user-facing MCP framework bugs, but not for protocol design claims.

### Tier 3: Opportunistic / Verify Only

`Aider-AI/aider`, `microsoft/autogen`, `seleniumbase/SeleniumBase`, `steel-dev/steel-browser`, `morph-labs/morph-python-sdk`, `morph-labs/morph-typescript-sdk`, `docker/mcp-gateway`, `docker/mcp-registry`, Modal examples.

Reasons: lower immediate Paperclip fit, stale or unclear public contribution path, examples/catalog focus, or weaker direct AI-native runtime surface.

## Cycle 3: Critique / Decision

3-subagent critique requested before tracker changes:

- fact-check / duplicate: accept update only if queue counts are explicitly marked as parent GraphQL counts and strategic high-churn repos are not presented as PR-ready;
- process-gate: no upstream PR/comment, no bug-level `PR-READY`, runner/repro gate marked not applicable because this is repo-level universe work;
- actionability: next cycle must use repo-level actions, not vague "popular repo scan".

Final `next_status` for this block: `WATCH`.

Repo-level actions:

| Action | Repo(s) | Reason |
|---|---|---|
| promote | `pydantic/pydantic-ai`, `OpenHands/OpenHands`, `e2b-dev/E2B`, `cline/cline`, `trycua/cua`, `microsoft/playwright-mcp`, `langchain-ai/deepagents` | Best current balance of Paperclip relevance, test surface, and actionable contribution lanes. |
| update scope | `openai/codex`, `anomalyco/opencode`, `google-gemini/gemini-cli`, `daytonaio/daytona` | Strategic Tier 1 but watch-first due to churn and duplicate risk. |
| watch | Tier 2 set above | Useful universe, but requires comment-first or stronger repo-specific gates. |
| demote / verify only | Tier 3 set above | Not good default target for the next hourly scouting block. |
| create repo-card | [repo cards](repo-cards-ai-native-2026-05-27.md) | Next cycle now has concrete scan lanes, runner need, duplicate risk and promotion gate per repo. |

Critique corrections applied:

- high-churn queue counts refreshed for `openai/codex`, `anomalyco/opencode`, `trycua/cua`, `daytonaio/daytona`;
- `OpenHands/OpenHands` and `PrefectHQ/fastmcp` recorded as canonical names;
- Docker and Morph buckets split into explicit repos;
- Tier labels framed as repo-level scouting priority, not validated candidate bugs.

## Next Hourly Block

Start from Tier 1A and pick one repo-level lane, not a random top-star repo:

1. `pydantic/pydantic-ai`: provider/tool/MCP regression queue, low-cost tests.
2. `e2b-dev/E2B`: SDK/session/docs bug lane with smaller queue.
3. `microsoft/playwright-mcp`: browser tool protocol regressions with tiny backlog.
4. `OpenHands/OpenHands`: skills/workspace/sandbox issues, comment-first for design ambiguity.
5. `trycua/cua`: computer-use driver/replay lane, only with current native/runner repro.
6. Use [repo cards](repo-cards-ai-native-2026-05-27.md) to select scan lane and promotion gate.
