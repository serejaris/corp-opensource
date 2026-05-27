# AI-native popular repo universe, cycle 24, 2026-05-27

Live state: parent `gh repo view` pass на 2026-05-27 17:39 UTC.
Tracker: [serejaris/corp-opensource#52](https://github.com/serejaris/corp-opensource/issues/52).

Upstream actions в этом блоке: 0 PR, 0 comments.

Final `next_status`: `WATCH`.

Cycle 24 является только repo-level scouting/scope update. Конкретный upstream bug, issue или PR target не выбран. Tracker `#52` остаётся `OPEN` с labels `needs-subagents`, `repo-scouting`, `ai-agent`, `needs-repro`; parent check зафиксирован на `2026-05-27T17:27:10Z`. Six-subagent scouting завершён; parent `gh repo view` gate пройден. Runner/repro здесь N/A, потому что нет конкретного repro target. Этот цикл не разрешает upstream comment или PR.

`open-source-bug-scouting` и `open-source-pr-workflow` skills в текущей среде не установлены. Subagent tools доступны и использованы: шесть ролей для discovery/fit, затем три роли critique.

## Cycle 1: Research / Discovery

Six role split:

- control-plane / terminal coding agents;
- MCP protocol/runtime/tools;
- browser automation / computer-use;
- sandbox/session orchestration/runners;
- skills/capabilities/plugin ecosystems;
- eval/harness/benchmark repos.

New or refreshed repos that were not fully represented in cycle 23:

- MCP official cluster: `modelcontextprotocol/registry`, `modelcontextprotocol/inspector`, `modelcontextprotocol/servers`, `modelcontextprotocol/modelcontextprotocol`;
- agent framework / capabilities: `openai/openai-agents-python`, `ComposioHQ/composio`, `promptfoo/promptfoo`, `microsoft/semantic-kernel`;
- terminal/control-plane: `aaif-goose/goose`, `QwenLM/qwen-code`, `gptme/gptme`, `BloopAI/vibe-kanban`, `awslabs/cli-agent-orchestrator`, `Open-ACP/OpenACP`;
- eval/harness: `SWE-agent/SWE-agent`, `SWE-agent/mini-swe-agent`, `SWE-bench/SWE-bench`, `ServiceNow/BrowserGym`, `xlang-ai/OSWorld`;
- browser/computer-use: `Skyvern-AI/skyvern`, `microsoft/Webwright`, `browserbase/mcp-server-browserbase`;
- infra-adjacent: `dagger/dagger`, `devcontainers/cli`.

## Parent Live Gates

Authoritative parent table uses `gh repo view` GraphQL counts. Subagent search counts sometimes included combined issue+PR totals or partial data under rate limits.

| Repo | Stars | Pushed | Release | Open issues | Open PRs | Decision |
|---|---:|---|---|---:|---:|---|
| `openai/openai-agents-python` | 26705 | 2026-05-26 | `v0.17.4` | 45 | 68 | promote Tier 1A, unit-test SDK/runtime lane |
| `modelcontextprotocol/registry` | 6865 | 2026-05-25 | `v1.7.9` | 80 | 16 | Tier 1B strategic MCP watch |
| `modelcontextprotocol/inspector` | 9892 | 2026-05-27 | `0.21.2-hotfix-3` | 167 | 104 | Tier 1B strategic MCP watch |
| `modelcontextprotocol/servers` | 86332 | 2026-05-27 | `2026.1.26` | 293 | 233 | Tier 1B watch, high duplicate risk |
| `modelcontextprotocol/modelcontextprotocol` | 8239 | 2026-05-27 | `2025-11-25` | 129 | 113 | spec-first watch, comment-first only |
| `aaif-goose/goose` | 45956 | 2026-05-27 | `v1.35.0` | 130 | 74 | Tier 1B / PR-open watch |
| `ComposioHQ/composio` | 28482 | 2026-05-27 | `@composio/claude-agent-sdk@0.9.2` | 28 | 83 | Tier 2 active watch; package/path-specific only |
| `promptfoo/promptfoo` | 21658 | 2026-05-27 | `code-scan-action-0.1.6` | 75 | 244 | Tier 2 active watch; high PR collision risk |
| `SWE-agent/SWE-agent` | 19340 | 2026-05-25 | `v1.1.0` | 28 | 26 | Tier 2 active watch |
| `SWE-agent/mini-swe-agent` | 4612 | 2026-05-25 | `v2.3.0` | 17 | 9 | Tier 2 active watch; cross-repo duplicate gate |
| `Skyvern-AI/skyvern` | 21755 | 2026-05-27 | `v1.0.36` | 34 | 121 | Tier 2 active watch |
| `BloopAI/vibe-kanban` | 26546 | 2026-04-24 | `v0.1.44-20260424091429` | 376 | 151 | Tier 2 watch, strong fit but noisy |
| `QwenLM/qwen-code` | 24707 | 2026-05-27 | `v0.16.2` | 650 | 153 | Tier 2/3 verify-first |
| `gptme/gptme` | 4310 | 2026-05-27 | `v0.31.0` | 11 | 1 | Tier 2/3 verify-first |
| `awslabs/cli-agent-orchestrator` | 638 | 2026-05-26 | `v2.1.1` | 23 | 10 | Tier 2/3 verify-first |
| `Open-ACP/OpenACP` | 401 | 2026-05-18 | `v2026.518.2` | 4 | 1 | Tier 2/3 ecosystem watch |
| `microsoft/Webwright` | 2504 | 2026-05-27 | `none` | 3 | 6 | Tier 3 scope/watch |
| `ServiceNow/BrowserGym` | 1231 | 2026-03-17 | `v0.14.3` | 18 | 15 | Tier 3 benchmark watch |
| `dagger/dagger` | 15880 | 2026-05-27 | `v0.21.0` | 45 | 55 | Tier 3 infra-adjacent |
| `devcontainers/cli` | 2735 | 2026-05-21 | `none` | 225 | 59 | Tier 3 infra-adjacent |

## Cycle 2: Triage / Fit

### Tier 1A: Promote

| Repo | Paperclip layer | Operating mode |
|---|---|---|
| `openai/openai-agents-python` | skills/capabilities, tools, handoffs, tracing, SDK runtime | Active scouting для узких SDK/runtime/tool-call regressions с unit-test surface. Перед repro фиксировать точный commit/version из-за быстрого release cadence. |

Carry forward from cycle 23: `pydantic/pydantic-ai`, `OpenHands/OpenHands`, `e2b-dev/E2B`, `cline/cline`, `trycua/cua`, `microsoft/playwright-mcp`, `langchain-ai/deepagents`.

### Tier 1B: Strategic Watch

| Repo | Why | Operating mode |
|---|---|---|
| `modelcontextprotocol/registry` | canonical MCP discovery/trust/metadata layer | Только targeted triage; registry API/metadata issues отделять от spec и server implementation issues. |
| `modelcontextprotocol/inspector` | MCP devloop/debug UI/runtime | Watch; PR search по affected UI/runtime area перед любым статусом выше `LEAD`. |
| `modelcontextprotocol/servers` | canonical server hub | Watch; 293 issues / 233 PRs означают высокий duplicate-race risk. |
| `modelcontextprotocol/modelcontextprotocol` | MCP spec/docs canonical repo | Spec-first reference; только comment-first. |
| `aaif-goose/goose` | local terminal/tool agent with existing local PR lane | PR-open watch; не искать новый goose PR, пока `#9447` открыт, а `#9136/#9332` остаются comment-first gated. |

Carry forward strategic watch: `openai/codex`, `anomalyco/opencode`, `google-gemini/gemini-cli`, `daytonaio/daytona`.

### Tier 2: Active Watch / Verify Before Candidate

| Repo | Layer | Mode |
|---|---|---|
| `ComposioHQ/composio` | tool registry / auth / integrations | Только package/path-specific; default branch `next`, release tag может описывать один package, а не весь monorepo. |
| `promptfoo/promptfoo` | eval/harness / provider assertions / plugins | Сильный fit, но 244 open PRs требуют targeted PR search перед comment или code. |
| `SWE-agent/SWE-agent` | coding-agent harness / runner | Active watch для benchmark runner, Docker/env и docs drift; cross-check с `mini-swe-agent`. |
| `SWE-agent/mini-swe-agent` | compact terminal-agent harness | Active watch; не считать независимым от `SWE-agent/SWE-agent` без cross-repo search. |
| `Skyvern-AI/skyvern` | browser/vision/RPA agent | Active watch; только distinct browser-agent surface, уже не покрытый `browser-use`, `stagehand`, `playwright-mcp`, `trycua`. |
| `BloopAI/vibe-kanban` | Paperclip-like control-plane / agent session UI | Сильный strategic fit, но queue noise высокий; comment-first до проверки contribution surface. |
| `QwenLM/qwen-code` | terminal coding agent | Verify-first из-за 650 issues / 153 PRs и вероятных duplicate reports. |
| `gptme/gptme` | lower-noise terminal coding agent | Verify-first; низкий PR count полезен только при хорошем current maintainer rhythm. |
| `awslabs/cli-agent-orchestrator` | multi-agent CLI control-plane | Verify-first; низкая popularity, но сильная control-plane relevance. |
| `Open-ACP/OpenACP` | ACP bridge/control-plane | Ecosystem watch; низкий duplicate risk, но low evidence surface. |

### Tier 3: Scope / Reference Watch

`microsoft/Webwright`, `ServiceNow/BrowserGym`, `dagger/dagger`, `devcontainers/cli`, `SWE-bench/SWE-bench`, `xlang-ai/OSWorld`, `microsoft/semantic-kernel`, `browserbase/mcp-server-browserbase`.

Причина: это полезная universe, но не default PR-hunt lanes. Для продвижения нужен concrete reproducible issue, maintainer signal или direct Paperclip runner/browser/eval failure.

## Cycle 3: Critique / Decision

3-subagent critique:

- fact-check / duplicate: repo-level freshness или stars не доказывают contribution readiness. MCP official repos нужно разделять по surface: spec, registry, inspector, server implementation. Не писать `modelcontextprotocol/specification`; использовать `modelcontextprotocol/modelcontextprotocol`.
- process-gate: порядок соблюдён только для repo-level scope update. Для любого upstream status-changing action gate не пройден, потому что нет конкретного issue/bug/PR target.
- actionability: final status остаётся `WATCH`; создать/обновить repo cards, продвигать только repo-level scan lanes и не выбирать issue в этом цикле.

Final repo-level actions:

| Action | Repo(s) | Reason |
|---|---|---|
| promote | `openai/openai-agents-python` | Strong AI-native SDK/capabilities fit, manageable issue count, clear testable runtime surfaces. |
| update scope | MCP official cluster: `registry`, `inspector`, `servers`, `modelcontextprotocol` | Strategic layer for Paperclip tool discovery/protocol correctness, but split surfaces and use comment-first/spec gates. |
| update scope | `aaif-goose/goose` | Existing PR-open and comment-first lanes make it Tier 1B watch, not fresh PR hunting. |
| watch | `ComposioHQ/composio`, `promptfoo/promptfoo`, `SWE-agent/SWE-agent`, `SWE-agent/mini-swe-agent`, `Skyvern-AI/skyvern`, `BloopAI/vibe-kanban` | Strong fit, but needs repo-specific gates and duplicate scans before candidate work. |
| verify-first | `QwenLM/qwen-code`, `gptme/gptme`, `awslabs/cli-agent-orchestrator`, `Open-ACP/OpenACP` | Potentially useful, but contribution rhythm and duplicate risk need validation. |
| scope/reference watch | `microsoft/Webwright`, `ServiceNow/BrowserGym`, `dagger/dagger`, `devcontainers/cli` | Useful reference or infra-adjacent lanes, not default contribution targets. |

Guardrail: перед любым будущим status-changing upstream comment/PR нужно заново пройти parent live gates для конкретного target и fresh 3-subagent critique: factology/duplicates, process gates, actionability.

## Next Hourly Block

Repo-level lane order:

1. `openai/openai-agents-python`: сделать concrete scan вокруг tool-call/runtime/handoff regressions с unit tests; без issue-level duplicate search и regression card PR не открывать.
2. MCP official cluster: начинать с `registry` или `inspector`, а не со всех четырёх repos одновременно. `modelcontextprotocol/modelcontextprotocol` использовать только как spec reference/comment-first lane.
3. `pydantic/pydantic-ai`, `microsoft/playwright-mcp`, `langchain-ai/deepagents`: remain good Tier 1A fallbacks from cycle 23.
4. `ComposioHQ/composio`, `promptfoo/promptfoo`, `SWE-agent` repos, `Skyvern-AI/skyvern`, `BloopAI/vibe-kanban`: только active watch, пока не пройдены repo-specific gates.
5. `BrowserGym`, `Webwright`, `dagger`, `devcontainers/cli`: только reference/infra watch, если нет direct Paperclip runner/browser/eval failure.
