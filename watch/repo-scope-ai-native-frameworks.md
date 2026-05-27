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
| `google/adk-python` | Issue `#5864` active on `2026-05-27`; repo has contribution guide, PR template, unit tests, tox, and recent optional-dependency import PRs | Agent Development Kit: agent runtime/framework, code execution, memory, evaluation, Vertex AI integrations. Good for dependency-boundary and harness reliability bugs when repo gates allow. |
| `openai/openai-agents-python` | `26705` stars, pushed `2026-05-26`; parent count `45` open issues / `68` open PRs on `2026-05-27` | Agent SDK/runtime with tools, handoffs, tracing and testable unit-level surfaces. Good for narrow runtime/tool-call regressions after exact commit/version and duplicate gate. |

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
| `PrefectHQ/fastmcp` | Popular MCP framework/runtime; less spec-canonical than official SDKs, good for user-facing repro bugs. |
| `modelcontextprotocol/python-sdk` | Official SDK, but parent count `251` open issues / `241` open PRs and spec churn. |
| `modelcontextprotocol/typescript-sdk` | Official SDK, but parent count `222` open issues / `207` open PRs and duplicate-heavy PR flow. |
| `langchain-ai/langgraph` | Active graph/runtime ecosystem; crowded, fast-moving, only crisp regressions. |
| `CopilotKit/CopilotKit` | Frontend stack/protocol for agents and generative UI; continue existing PR/watch lanes. |
| `continuedev/continue` | IDE/CLI/CI agent workflow; verify product-direction fit before heavy scouting. |
| `crewAIInc/crewAI` | Popular orchestration framework, but PR queue is saturated; comment-first. |
| `ComposioHQ/composio` | Tool registry/auth/integration monorepo; package/path-specific only because default branch is `next` and release tags can describe one package. |
| `promptfoo/promptfoo` | Eval/harness/provider assertion and plugin surface; high PR collision risk, targeted PR search required. |
| `SWE-agent/SWE-agent` | Coding-agent harness and runner; useful for benchmark runner, Docker/env and docs drift bugs. |
| `SWE-agent/mini-swe-agent` | Compact terminal-agent harness; cross-check with `SWE-agent/SWE-agent` before treating a candidate as independent. |
| `Skyvern-AI/skyvern` | Browser/vision/RPA agent; take only distinct browser-agent issues not already covered by current browser lanes. |
| `BloopAI/vibe-kanban` | Paperclip-like control-plane for coding agents; strong fit but noisy queue, comment-first until contribution surface is validated. |
| `QwenLM/qwen-code` | Terminal coding agent; verify-first due to high issue/PR volume and likely duplicate reports. |
| `gptme/gptme` | Lower-noise terminal agent; verify maintainer rhythm before promotion. |
| `awslabs/cli-agent-orchestrator` | Multi-agent CLI control-plane; lower popularity but relevant control-plane watch. |
| `Open-ACP/OpenACP` | ACP bridge/control-plane; early ecosystem watch with low evidence surface. |

## Tier 3: Opportunistic / Verify Only

These can become Tier 1 only after identity and repo-health validation.

| Repo | Why verify first |
|---|---|
| `openclaw/openclaw` | Search reports unusually high stars; verify releases, commits, issue quality, and maintainer loop before trusting. |
| `Gitlawb/openclaude` | OpenClaude-like target, but identity/canonical status needs validation. |
| `Infisical/agent-vault` | Relevant credential proxy/vault for agents, smaller/newer repo; validate activity and test harness. |
| `HKUDS/OpenHarness` | Appears in agent-harness search; validate real usage and maintainership. |
| `langchain-ai/deepagentsjs` | JS sibling of DeepAgents; validate maturity and issue quality. |
| `OpenHands/software-agent-sdk` | Suggested by OpenHands maintainers; validate separately from monorepo issues. |
| `OpenHands/OpenHands-CLI` | Suggested by OpenHands maintainers; validate separately from monorepo issues. |
| `Aider-AI/aider` | Mature terminal pair-programming baseline, but slower current release signal and lower immediate Paperclip payoff. |
| `microsoft/autogen` | Large Microsoft framework; lower current push freshness and high backlog/triage risk. |
| `seleniumbase/SeleniumBase` | Strong browser automation substrate, but not AI-native by default. |
| `steel-dev/steel-browser` | Relevant smaller browser sandbox; verify maintainer rhythm before promotion. |
| `morph-labs/morph-python-sdk` | Very low public stars/activity; keep only as Morph-specific watch. |
| `morph-labs/morph-typescript-sdk` | Very low public stars/activity; keep only as Morph-specific watch. |
| `docker/mcp-gateway` | Useful MCP distribution/gateway watch, but less direct than SDK/runtime work. |
| `docker/mcp-registry` | Registry/catalog queue is noisy; only exact metadata validation bugs. |
| `microsoft/Webwright` | Browser-agent framework; reference/watch until contribution docs and release maturity are clearer. |
| `ServiceNow/BrowserGym` | Web-agent benchmark environment; research/benchmark watch, not default PR lane. |
| `dagger/dagger` | Execution graph/container infra; only direct agent-runner breakages belong here. |
| `devcontainers/cli` | Workspace/devcontainer CLI; only direct Paperclip runner/workspace failures belong here. |

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

Latest repo-universe update: [cycle 24](ai-native-popular-repos-paperclip-scouting-2026-05-27-cycle-24-repo-universe.md). Repo-card queue: [repo cards 2026-05-27](repo-cards-ai-native-2026-05-27.md).
