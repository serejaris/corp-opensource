# Paperclip-adjacent repo map, 2026-05-27

Live state: GitHub checks via `gh` on 2026-05-27.
Scope: suitable repositories for Paperclip-like scouting, not bug targets.

## Anchor

| Repo | Stars | Role |
|---|---:|---|
| [paperclipai/paperclip](https://github.com/paperclipai/paperclip) | 67.9k | Anchor: company/control-plane for AI agents; goals, tasks, budgets, heartbeats, adapters, skills, workspaces, audit. |

## Core repos

| Rank | Repo | Stars | Paperclip layer |
|---:|---|---:|---|
| 1 | [anomalyco/opencode](https://github.com/anomalyco/opencode) | 166.1k | Terminal coding-agent runtime, sessions, commands, adapters. |
| 2 | [OpenHands/OpenHands](https://github.com/OpenHands/OpenHands) | 75.0k | Dev-agent runtime, workspace, sandbox, skills. |
| 3 | [bytedance/deer-flow](https://github.com/bytedance/deer-flow) | 69.8k | Long-horizon tasks, memory, tools, subagents. |
| 4 | [cline/cline](https://github.com/cline/cline) | 62.4k | IDE/CLI coding agent, MCP, terminal policy, human-in-loop. |
| 5 | [aaif-goose/goose](https://github.com/aaif-goose/goose) | 46.0k | Local agent runtime, MCP subprocesses, tool lifecycle. |
| 6 | [multica-ai/multica](https://github.com/multica-ai/multica) | 33.6k | Managed coding agents as teammates; task control-plane. |
| 7 | [BloopAI/vibe-kanban](https://github.com/BloopAI/vibe-kanban) | 26.5k | Board/orchestration surface for coding agents. |
| 8 | [trycua/cua](https://github.com/trycua/cua) | 17.1k | Computer-use infra, desktop sandbox, MCP driver. |

## Adjacent infrastructure

| Rank | Repo | Stars | Paperclip layer |
|---:|---|---:|---|
| 1 | [browser-use/browser-use](https://github.com/browser-use/browser-use) | 95.9k | Browser worker/runtime, CDP/session/tool lifecycle. |
| 2 | [openai/codex](https://github.com/openai/codex) | 86.3k | BYO coding-agent backend. |
| 3 | [anthropics/claude-code](https://github.com/anthropics/claude-code) | 127.0k | External coding-agent backend. |
| 4 | [ChromeDevTools/chrome-devtools-mcp](https://github.com/ChromeDevTools/chrome-devtools-mcp) | 42.0k | DevTools MCP surface for agents. |
| 5 | [microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp) | 33.1k | Browser automation through MCP. |
| 6 | [continuedev/continue](https://github.com/continuedev/continue) | 33.4k | IDE agent, CI/source-controlled checks, MCP/model adapters. |
| 7 | [ComposioHQ/composio](https://github.com/ComposioHQ/composio) | 28.5k | Tool/auth/action layer for agents. |
| 8 | [pydantic/pydantic-ai](https://github.com/pydantic/pydantic-ai) | 17.3k | Agent framework, MCP/toolset integration. |
| 9 | [modelcontextprotocol/python-sdk](https://github.com/modelcontextprotocol/python-sdk) | 23.1k | Official MCP Python server/client layer. |
| 10 | [modelcontextprotocol/typescript-sdk](https://github.com/modelcontextprotocol/typescript-sdk) | 12.5k | Official MCP TypeScript protocol/runtime layer. |
| 11 | [langchain-ai/open-swe](https://github.com/langchain-ai/open-swe) | 9.9k | Async SWE/coding task executor. |

## Watch with caution

| Repo | Reason |
|---|---|
| [langchain-ai/langchain](https://github.com/langchain-ai/langchain) | Very broad agent framework; high issue noise, weaker Paperclip control-plane fit. |
| [langgenius/dify](https://github.com/langgenius/dify) | Large product/workflow platform; useful SaaS ideas, heavy contribution surface. |
| [langflow-ai/langflow](https://github.com/langflow-ai/langflow) | Visual workflow builder; app/UI noise higher than runtime learning. |
| [microsoft/autogen](https://github.com/microsoft/autogen) | Broad multi-agent framework; design churn. |
| [crewAIInc/crewAI](https://github.com/crewAIInc/crewAI) | Popular, but noisy issue flow and many duplicate/agent PRs. |
| [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | Interesting memory layer; watch for session-memory ideas. |
| [lobehub/lobehub](https://github.com/lobehub/lobehub) | Operator/team framing; less direct runtime contribution fit. |
| [ruvnet/ruflo](https://github.com/ruvnet/ruflo) | Swarm/orchestration framing; verify depth before treating as core. |
| [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) | Skills library; useful for capability model, weaker runtime surface. |
| [oraios/serena](https://github.com/oraios/serena) | Semantic code tools / MCP-like IDE layer; good watch candidate. |

## Avoid as primary targets

Guides, prompt dumps, awesome lists and course repos are useful for reading, but weak as primary repo targets:

- `x1xhlol/system-prompts-and-models-of-ai-tools`
- `Shubhamsaboo/awesome-llm-apps`
- `dair-ai/Prompt-Engineering-Guide`
- `microsoft/ai-agents-for-beginners`
- `shanraisshan/claude-code-best-practice`
- `awesome-*`, `*-guide`, `*-best-practice`

## Short call

Main repo set for the next Paperclip-adjacent scouting loop:

`paperclipai/paperclip`, `OpenHands/OpenHands`, `bytedance/deer-flow`, `multica-ai/multica`, `BloopAI/vibe-kanban`, `anomalyco/opencode`, `cline/cline`, `aaif-goose/goose`, `browser-use/browser-use`, `trycua/cua`, `modelcontextprotocol/python-sdk`, `modelcontextprotocol/typescript-sdk`, `microsoft/playwright-mcp`, `ChromeDevTools/chrome-devtools-mcp`.

