# Repo Scope: AI-native frameworks and harnesses

Last updated: 2026-05-27

## Purpose

Этот файл отвечает на вопрос: какие репозитории мы заранее считаем допустимым полем для open-source scouting.

Без этого агенты начинают искать “топовые репозитории” слишком широко. Для Ris цель уже: Paperclip-like / Hermes-like / Codex-like infrastructure.

## Scope Rule

Перед поиском багов сначала выбери repo из этого списка или добавь новый repo в `candidate / verify` с доказательствами:

- что это реальный AI-native framework/harness/runtime;
- что repo активен;
- что есть issues/PR loop;
- что есть test harness;
- что потенциальный вклад улучшает Paperclip/Hermes/Codex-style работу.

## Tier 0: Internal Reference, Not Upstream PR Targets

| Repo | Role |
|---|---|
| `~/Documents/GitHub/paperclip` | Our own control-plane/harness reference: tasks, agents, goals, plugins, skills. |
| `~/Documents/GitHub/corp-opensource` | Queue, watch notes, decisions, scouting evidence. |
| `~/Documents/GitHub/corp-server` | Runner/worker/container infra owner. |
| `~/Documents/GitHub/corp-hermes` / Hermes checkouts | Runtime reference for Telegram/Hermes/Codex failures. |

## Tier 1A: Core External Targets For Active Scouting

These are allowed for regular weekly scouting.

| Repo | Current live signal | Why in scope |
|---|---|---|
| `cline/cline` | `62376` stars, pushed `2026-05-27` | Coding-agent IDE/CLI runtime, MCP/tool/provider bugs, checkpoints, browser automation. |
| `OpenHands/OpenHands` | `74995` stars, pushed `2026-05-27` | AI-driven development platform, skills, resolver, sandbox, browser, self-hosting, worker lifecycle. |
| `pydantic/pydantic-ai` | `17327` stars, pushed `2026-05-27` | Typed agent framework, providers/tools/model settings, strong fixture/test culture. |
| `trycua/cua` | active bug queue on `2026-05-27` | Computer-use agent infrastructure, sandboxes, benchmarks, trajectory recording. |
| `langchain-ai/deepagents` | `23391` stars, pushed `2026-05-27` | Agent harness with LangChain maintainership; likely tool/memory/runtime tests. |
| `e2b-dev/E2B` | `12371` stars, pushed `2026-05-27` | Secure sandbox/runtime layer for agents. |
| `microsoft/playwright-mcp` | `33098` stars, pushed `2026-05-23`; parent count `5` open issues / `4` open PRs on `2026-05-27` | Canonical browser automation MCP server; small backlog and concrete tool/protocol regression surface. |
| `ChromeDevTools/chrome-devtools-mcp` | `41969` stars, pushed `2026-05-27`; parent count `65` open issues / `16` open PRs on `2026-05-27` | Chrome DevTools MCP browser/debugging runtime for coding agents; separate lane from Playwright MCP. |
| `PrefectHQ/fastmcp` | `25346` stars, pushed `2026-05-27`; parent count `218` open issues / `15` open PRs on `2026-05-27` | Popular MCP framework/runtime with local fixture-friendly client/server/tool surfaces; assignment-first and no spec-design PR without maintainer signal. |
| `google/adk-python` | Issue `#5864` active on `2026-05-27`; repo has contribution guide, PR template, unit tests, tox, and recent optional-dependency import PRs | Agent Development Kit: agent runtime/framework, code execution, memory, evaluation, Vertex AI integrations. Good for dependency-boundary and harness reliability bugs when repo gates allow. |
| `openai/openai-agents-python` | `26705` stars, pushed `2026-05-26`; parent count `45` open issues / `68` open PRs on `2026-05-27` | Agent SDK/runtime with tools, handoffs, tracing and testable unit-level surfaces. Good for narrow runtime/tool-call regressions after exact commit/version and duplicate gate. |
| `SWE-agent/mini-swe-agent` | `4617` stars, pushed `2026-05-25`; parent count `17` open issues / `9` open PRs on `2026-05-27` | Compact terminal-agent and eval/harness target; small enough for full issue sweep, but runner repro and duplicate search are required before upstream action. |

## Tier 1B: Strategic Watch, Not Blind PR Hunting

These repos are strategically important but too noisy for random bug hunting. Use only after strict live gates and a narrow repro.

| Repo | Current live signal | Why watch-first |
|---|---|---|
| `openai/codex` | `86281` stars, pushed `2026-05-27`; parent count `4944` open issues / `301` open PRs | Direct terminal coding-agent reference, but duplicate/churn risk is extreme. |
| `anomalyco/opencode` | `166083` stars, pushed `2026-05-27`; parent count `5265` open issues / `940` open PRs | Huge active terminal-agent ecosystem; repo already has multiple local upstream PR lanes open. |
| `google-gemini/gemini-cli` | `104656` stars, pushed `2026-05-27`; parent count `1198` open issues / `302` open PRs | Official Gemini CLI agent; corporate/high-churn process makes comment-first the default. |
| `daytonaio/daytona` | `72452` stars, pushed `2026-05-27`; parent count `275` open issues / `130` open PRs | Secure dev env / runner/session control-plane; high velocity and broad infra surface require tight scope. |
| `modelcontextprotocol/registry` | `6865` stars, pushed `2026-05-25`; parent count `80` open issues / `16` open PRs | Canonical MCP discovery/trust/metadata layer; keep separate from spec and server implementation bugs. |
| `modelcontextprotocol/inspector` | `9892` stars, pushed `2026-05-27`; parent count `167` open issues / `104` open PRs | MCP devloop/debug UI/runtime; useful but hotfix cadence requires affected-area PR search before candidate work. |
| `modelcontextprotocol/servers` | `86332` stars, pushed `2026-05-27`; parent count `293` open issues / `233` open PRs | Canonical server hub; strategic watch only because duplicate-race risk is high. |
| `modelcontextprotocol/modelcontextprotocol` | `8239` stars, pushed `2026-05-27`; parent count `129` open issues / `113` open PRs | MCP spec/docs reference; comment-first only for protocol semantics. |
| `aaif-goose/goose` | `45956` stars, pushed `2026-05-27`; parent count `130` open issues / `74` open PRs | Local terminal/tool agent; existing PR-open and comment-first lanes mean watch, not fresh PR hunting. |

## Tier 2: Watch / Comment-First Before Bug Scouting

These can produce good candidates, but the next step must be repo-specific gates and issue/PR search rather than broad PR hunting.

| Repo | Why watch first |
|---|---|
| `browser-use/browser-use` | Strong browser-agent fit, but parent count `66` open issues / `177` open PRs and high duplicate race. |
| `browserbase/stagehand` | AI browser SDK with active queue; avoid broad SDK PRs without maintainer confirmation. |
| `modelcontextprotocol/python-sdk` | Official SDK, but parent count `251` open issues / `241` open PRs and spec churn. |
| `modelcontextprotocol/typescript-sdk` | Official SDK, but parent count `222` open issues / `207` open PRs and duplicate-heavy PR flow. |
| `langchain-ai/langgraph` | Active graph/runtime ecosystem; crowded, fast-moving, only crisp regressions. |
| `CopilotKit/CopilotKit` | Frontend stack/protocol for agents and generative UI; continue existing PR/watch lanes. |
| `continuedev/continue` | IDE/CLI/CI agent workflow; verify product-direction fit before heavy scouting. |
| `crewAIInc/crewAI` | Popular orchestration framework, but PR queue is saturated; comment-first. |
| `ComposioHQ/composio` | Tool registry/auth/integration monorepo; package/path-specific only because default branch is `next` and release tags can describe one package. |
| `promptfoo/promptfoo` | Eval/harness/provider assertion and plugin surface; high PR collision risk, targeted PR search required. |
| `SWE-agent/SWE-agent` | Coding-agent harness and runner; useful for benchmark runner, Docker/env and docs drift bugs. |
| `SWE-agent/mini-swe-agent` | Promoted to Tier 1A after cycle 29; kept here only as a reminder to cross-check with `SWE-agent/SWE-agent` before treating a candidate as independent. |
| `Skyvern-AI/skyvern` | Browser/vision/RPA agent; take only distinct browser-agent issues not already covered by current browser lanes. |
| `BloopAI/vibe-kanban` | Paperclip-like control-plane for coding agents; [bounded scan](vibe-kanban-scouting-2026-05-27.md) kept `WATCH` because repo is sunsetting, README asks for discussion before PR, and maintainer rhythm is weak. `#3329` is lead-after-repro only; no PR hunting. |
| `QwenLM/qwen-code` | Terminal coding agent; verify-first due to high issue/PR volume and likely duplicate reports. |
| `gptme/gptme` | Lower-noise terminal agent; verify maintainer rhythm before promotion. |
| `awslabs/cli-agent-orchestrator` | Multi-agent CLI control-plane; lower popularity but relevant control-plane watch. |
| `Open-ACP/OpenACP` | ACP bridge/control-plane; early ecosystem watch with low evidence surface. |
| `ag-ui-protocol/ag-ui` | Agent UI/control-plane protocol; active but high churn, use for SDK/protocol conformance only after subsystem gate. |
| `mcp-use/mcp-use` | MCP app/client/server framework; useful integration surface, but compare against official SDK behavior before calling a bug. |
| `microsoft/agent-framework` | Microsoft Python/.NET agent framework; high backlog means exact package/subsystem gate only. |
| `mastra-ai/mastra` | TypeScript agent framework/workflows; useful watch but high issue/PR flow. |
| `langfuse/langfuse` | LLM tracing/eval/observability for agent workflows; avoid generic product support issues. |
| `comet-ml/opik` | LLM tracing/eval/observability; use only scoped SDK/integration/eval-runner bugs. |
| `confident-ai/deepeval` | LLM eval framework; relevant only when directly tied to agent/eval harness reliability. |
| `langchain-ai/openevals` | Smaller LangChain eval harness; low duplicate risk but verify maintainer rhythm before promotion. |
| `huggingface/smolagents` | Lightweight agent framework; significant but crowded, comment-first without concrete regression. |
| `microsoft/magentic-ui` | Experimental browser/local-file agent UI; intake and contribution surface need validation. |

## Tier 3: Opportunistic / Verify Only

These can become Tier 1 only after identity and repo-health validation.

| Repo | Why verify first |
|---|---|
| `openclaw/openclaw` | `NO-GO` after [#63](https://github.com/serejaris/corp-opensource/issues/63) / [watch](openclaw-openclaude-identity-scouting-2026-05-27.md): high identity/provenance and duplicate-race risk. Not proven noncanonical, but requires external canonical validation and manual process proof before any contribution lane. |
| `Gitlawb/openclaude` | `NO-GO` after [#63](https://github.com/serejaris/corp-opensource/issues/63) / [watch](openclaw-openclaude-identity-scouting-2026-05-27.md): explicit proprietary-derived-code provenance/license blocker; no PR/comment/repro lane without external legal/provenance clearance. |
| `Infisical/agent-vault` | Relevant credential proxy/vault for agents; bounded scan [#60](https://github.com/serejaris/corp-opensource/issues/60) / [watch](agent-vault-scouting-2026-05-27.md) kept final `WATCH`. Best lead `#194` is Codex/Agent Vault WebSocket proxy compatibility, but Agent Vault-owned patch surface is unproven until secret-free v0.22.0/current-main runner repro and owner-boundary gate. |
| `HKUDS/OpenHarness` | Active verify-first harness watch via [#61](https://github.com/serejaris/corp-opensource/issues/61) / [watch](openharness-scouting-2026-05-27.md). High AI-native fit, but current open queue is feature/discussion-heavy and PR queue is conflict-heavy; promote only after a reproducible current-main harness bug and duplicate gate. |
| `langchain-ai/deepagentsjs` | High-fit active watch via [#62](https://github.com/serejaris/corp-opensource/issues/62) / [watch](deepagentsjs-scouting-2026-05-27.md). Strongest uncovered lead is `#547` (`StateBackend` instance vs factory), but final status stayed `WATCH` because LangChain requires maintainer-approved solutions before PR and runner/current-main repro is still missing. |
| `OpenHands/software-agent-sdk` | Suggested by OpenHands maintainers; validate separately from monorepo issues. |
| `OpenHands/OpenHands-CLI` | Suggested by OpenHands maintainers; validate separately from monorepo issues. |
| `Aider-AI/aider` | Mature terminal pair-programming baseline, but slower current release signal and lower immediate Paperclip payoff. |
| `microsoft/autogen` | Large Microsoft framework; lower current push freshness and high backlog/triage risk. |
| `seleniumbase/SeleniumBase` | Strong browser automation substrate, but not AI-native by default. |
| `steel-dev/steel-browser` | Relevant smaller browser sandbox; verify maintainer rhythm before promotion. |
| `morph-labs/morph-python-sdk` | Very low public stars/activity; close generic scouting scope unless a Morph-specific target is requested. |
| `morph-labs/morph-typescript-sdk` | Very low public stars/activity; close generic scouting scope unless a Morph-specific target is requested. |
| `docker/mcp-gateway` | Useful MCP distribution/gateway reference, but demoted to exact validation/config bugs only. |
| `docker/mcp-registry` | Registry/catalog queue is noisy; demoted to exact metadata validation bugs only. |
| `microsoft/Webwright` | Browser-agent framework; reference/watch until contribution docs and release maturity are clearer. |
| `ServiceNow/BrowserGym` | Web-agent benchmark environment; research/benchmark watch, not default PR lane. |
| `dagger/dagger` | Execution graph/container infra; only direct agent-runner breakages belong here. |
| `devcontainers/cli` | Workspace/devcontainer CLI; only direct Paperclip runner/workspace failures belong here. |
| `openai/evals` | Historical eval reference; prefer newer eval targets unless a direct OpenAI Evals bug appears. |
| `anthropics/claude-code` | Canonical coding-agent reference but extreme issue/PR volume; no fresh scouting unless explicitly requested with an ultra-narrow repro lane. |

## Out Of Scope By Default

- Awesome lists, tutorials, installers, “use case collections”.
- Thin wrappers around another agent without tests.
- Star-spike repos with no real issues/releases/maintainers.
- Broad “build a new agent architecture” requests.
- Product docs unless docs directly unblock harness/runtime use.
- `google-gemini/gemini-cli`, unless a fresh bug has strong repro and no better Paperclip-like candidate is available.

## Candidate Bug Preferences

Good:

- tool/MCP execution mismatch;
- provider adapter request-shape bug;
- browser/computer-use replay or screenshot failure;
- sandbox lifecycle or cleanup bug;
- task/goal/checkpoint state corruption;
- agent permission/token boundary bug;
- eval/harness flake with deterministic fixture;
- memory/skills/workflow loading bug.

Bad:

- needs paid provider credentials only;
- requires Windows/macOS-only live environment unless runner exists;
- no test harness;
- already covered by competing PR;
- maintainer says “needs design discussion” and no small testable slice exists.

## Weekly Scope Update Contract

Weekly scouting must update this scope only when:

1. A new repo has live evidence: stars are not enough.
2. The repo has a clear relation to Paperclip/Hermes/Codex-style workflows.
3. The repo has at least one likely patchable issue class.
4. The repo is not just a tutorial/list/wrapper.

Record removed or demoted repos with a reason.

Latest repo-universe update: [OpenClaw/OpenClaude identity scan](openclaw-openclaude-identity-scouting-2026-05-27.md), after [cycle 29](ai-native-popular-repos-paperclip-scouting-2026-05-27-cycle-29-repo-universe.md). Repo-card queue: [repo cards 2026-05-27](repo-cards-ai-native-2026-05-27.md).
