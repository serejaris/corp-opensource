# Repo cards: AI-native universe, 2026-05-27

Source: [cycle 23 repo-universe update](ai-native-popular-repos-paperclip-scouting-2026-05-27-cycle-23-repo-universe.md), [cycle 24 repo-universe update](ai-native-popular-repos-paperclip-scouting-2026-05-27-cycle-24-repo-universe.md), [cycle 25 openai-agents scan](ai-native-popular-repos-paperclip-scouting-2026-05-27-cycle-25-openai-agents.md), [cycle 27 SWE-agent mini scan](ai-native-popular-repos-paperclip-scouting-2026-05-27-cycle-27-swe-mini.md), [cycle 28 repo-universe update](ai-native-popular-repos-paperclip-scouting-2026-05-27-cycle-28-repo-universe.md), [cycle 29 repo-universe update](ai-native-popular-repos-paperclip-scouting-2026-05-27-cycle-29-repo-universe.md), [ChromeDevTools issue-level scan](chrome-devtools-mcp-scouting-2026-05-27.md), [FastMCP issue-level scan](fastmcp-scouting-2026-05-27.md), [AG-UI issue-level scan](ag-ui-scouting-2026-05-27.md), [mcp-use issue-level scan](mcp-use-scouting-2026-05-27.md), [Mastra issue-level scan](mastra-scouting-2026-05-27.md) и umbrella tracker [#52](https://github.com/serejaris/corp-opensource/issues/52).

Purpose: make the next bounded repo-scouting block concrete. These are repo-level cards, not bug candidates.

| Repo | Tier | Next action | Scan lane | Duplicate risk | Runner need | Last checked | Promotion gate |
|---|---|---|---|---|---|---|---|
| `pydantic/pydantic-ai` | 1A | promote | recent provider/tool/MCP regressions with test surface | medium-high | local/runner only after issue candidate | 2026-05-27 17:20 UTC | issue-level duplicate sweep + regression card |
| `OpenHands/OpenHands` | 1A | promote | skills/workspace/sandbox issues; maintainer-direction lanes | high | Docker/runner likely for repro | 2026-05-27 17:20 UTC | comment-first if roadmap ambiguous |
| `e2b-dev/E2B` | 1A | promote repo-level only | SDK/session/docs bugs with low cloud dependency; not a revival of closed `#1354` | low-medium | runner only for live sandbox/session repro | 2026-05-27 17:20 UTC | narrow repro not requiring private secrets |
| `cline/cline` | 1A | promote | deterministic extension/CLI/MCP/provider bugs | high | local checkout, possible extension harness | 2026-05-27 17:20 UTC | Linear/linked PR/duplicate gate clean |
| `trycua/cua` | 1A | promote | computer-use driver/replay/benchmark issues | high | native/runner required for platform bugs | 2026-05-27 17:24 UTC | current platform repro before PR |
| `microsoft/playwright-mcp` | 1A | watch / maintainer-direction | protocol/tool behavior regressions with tiny fixture; `#1635` first candidate, `#1636` secondary watch | medium-low but search API partially rate-limited | browser/CDP runner for `#1635`; long-run runner for `#1636` | 2026-05-27 18:25 UTC | wait maintainer approval/assignment; then repeat duplicate search and runner-backed repro |
| `langchain-ai/deepagents` | 1A | watch | filesystem/subagent/tool-call harness regressions | medium | local tests likely enough | 2026-05-27 18:00 UTC | raw provider payload needed before PR on Qwen/LiteLLM tool-call id lane |
| `openai/codex` | 1B | watch | churn, linked PRs, duplicate risk only | extreme | repo-specific | 2026-05-27 17:24 UTC | maintainer signal + ultra-narrow repro |
| `anomalyco/opencode` | 1B | watch | churn, existing PR lane interaction, duplicate risk | extreme | local tests likely, but avoid piling on | 2026-05-27 17:24 UTC | no new candidate promotion this cycle unless distinct stable signal |
| `google-gemini/gemini-cli` | 1B | watch | corporate/high-churn terminal-agent issues | high | local tests likely | 2026-05-27 17:20 UTC | maintainer ack + no competing PR |
| `daytonaio/daytona` | 1B | watch | runner/session lifecycle, no broad infra PRs | high | runner likely | 2026-05-27 17:24 UTC | sharply bounded repro + duplicate gate |
| `browser-use/browser-use` | 2 | update scope | comment-first eligible only for low-duplicate browser regressions | high | browser runtime | 2026-05-27 17:20 UTC | no speculative PR |
| `browserbase/stagehand` | 2 | update scope | watch SDK act/extract/observe regressions | high | browser runtime | 2026-05-27 17:20 UTC | maintainer confirmation before broad SDK PR |
| `PrefectHQ/fastmcp` | 1A | active watch | user-facing MCP framework bugs; see issue-level scan below | medium-high | local/runner MRE for issue candidates; Windows needed for SSE orphan-task lane | 2026-05-27 19:35 UTC | current-main MRE + fresh duplicate gate + assignment-first; avoid spec-design PRs |
| `modelcontextprotocol/python-sdk` | 2 | update scope | spec-aligned transport/client/server regressions | high | local tests likely | 2026-05-27 17:20 UTC | comment-first and duplicate sweep |
| `modelcontextprotocol/typescript-sdk` | 2 | update scope | protocol compliance and transport behavior | high | local tests likely | 2026-05-27 17:20 UTC | fresh competing PR scan |
| `google/adk-python` | 2 | update scope | docs/tests/eval regressions with clear maintainer path | high | local tests likely | 2026-05-27 17:20 UTC | no PR while clarification/request labels block |
| `continuedev/continue` | 2 | update scope | integration/config bugs after product-direction check | medium-high | local tests likely | 2026-05-27 17:16 UTC | verify current behavior on main |
| `crewAIInc/crewAI` | 2 | update scope | security/serialization/docs correctness | high | local tests likely | 2026-05-27 17:16 UTC | avoid broad features |
| `langchain-ai/langgraph` | 2 | update scope | crisp runtime regressions only | very high | local tests/server harness | 2026-05-27 17:16 UTC | linked issue and patch surface clear |
| `Aider-AI/aider` | 3 | demote | opportunistic only | medium | local | 2026-05-27 17:16 UTC | fresh external signal or user request |
| `microsoft/autogen` | 3 | demote | opportunistic only | high | local | 2026-05-27 17:16 UTC | fresh narrow bug with maintainer path |
| `seleniumbase/SeleniumBase` | 3 | demote | direct browser automation regressions only | low | browser runtime | 2026-05-27 17:20 UTC | clear agent-reliability connection |
| `steel-dev/steel-browser` | 3 | demote | verify maintainer rhythm first | medium | browser runtime | 2026-05-27 17:20 UTC | promote only after responsiveness signal |
| `morph-labs/morph-python-sdk` | 3 | demote | Morph-specific watch only | low | cloud account likely | 2026-05-27 17:18 UTC | direct user/requested target |
| `morph-labs/morph-typescript-sdk` | 3 | demote | Morph-specific watch only | low | cloud account likely | 2026-05-27 17:18 UTC | direct user/requested target |
| `docker/mcp-gateway` | 3 | demote | gateway validation/config only | high | Docker runtime | 2026-05-27 17:18 UTC | narrow repro and low duplicate risk |
| `docker/mcp-registry` | 3 | demote | registry metadata validation only | very high | usually no runner | 2026-05-27 17:18 UTC | exact validation bug |

## Cycle 24/25 overlay

Эти rows явно добавляют `avoid_pr_hunting`. Это repo-level cards, не bug candidates.

| Repo | Tier | Next action | Scan lane | Duplicate risk | Runner need | Last checked | avoid_pr_hunting | Promotion gate |
|---|---|---|---|---|---|---|---|---|
| `openai/openai-agents-python` | 1A | watch | SDK/runtime/tool-call/handoff/realtime regressions with unit-test surface | high | local tests first; realtime leads may need runner/live API gate | 2026-05-27 17:57 UTC | fixture-only | fresh current-main repro + clean duplicate/competing-PR gate + regression card |
| `modelcontextprotocol/registry` | 1B | update scope | registry metadata/API/discovery/trust | medium | local Go tests likely | 2026-05-27 17:39 UTC | comment-first | separate registry bugs from spec/server bugs |
| `modelcontextprotocol/inspector` | 1B | update scope | MCP inspector UI/runtime/auth/transport debugging | high | local/browser runtime likely | 2026-05-27 17:39 UTC | comment-first | affected-area PR search after hotfix cadence check |
| `modelcontextprotocol/servers` | 1B | watch | server implementation/catalog regressions | very high | repo-specific | 2026-05-27 17:39 UTC | yes | no PR without open PR/recent timeline sweep |
| `modelcontextprotocol/modelcontextprotocol` | 1B | watch | MCP spec/docs semantics | high | usually no runner | 2026-05-27 17:39 UTC | comment-first | spec issue discussion before code/docs PR |
| `aaif-goose/goose` | 1B | watch | existing PR/follow-up lanes only | medium-high | repo-specific | 2026-05-27 17:39 UTC | yes | no new goose PR while `#9447` is open and `#9136/#9332` remain gated |
| `ComposioHQ/composio` | 2 | update scope | package-specific tool/auth/integration bugs | medium-high | local tests likely | 2026-05-27 17:39 UTC | comment-first | package/path named, default branch `next` checked |
| `promptfoo/promptfoo` | 2 | update scope | provider/evaluator/assertion/plugin regressions | very high | local tests likely | 2026-05-27 17:39 UTC | comment-first | targeted PR search by provider/evaluator area |
| `SWE-agent/SWE-agent` | 2 | update scope | benchmark runner, Docker/env, docs drift | medium | Docker/runner likely | 2026-05-27 17:39 UTC | fixture-only | cross-check `mini-swe-agent` issues/PRs |
| `SWE-agent/mini-swe-agent` | 1A | active compact harness lane | compact terminal-agent harness regressions: `#826` timeout child processes ([#53](https://github.com/serejaris/corp-opensource/issues/53)), `#829` bash-vs-dash runner, tool-call parser FormatError; small enough for full issue sweep | medium | runner for timeout/process-tree repro before upstream action | 2026-05-27 19:35 UTC | fixture-only | runner repro + fresh duplicate search + regression card; no upstream action from repo-level cycle |
| `Skyvern-AI/skyvern` | 2 | watch | browser/vision/RPA agent regressions | high | browser/service stack likely | 2026-05-27 17:39 UTC | comment-first | distinct from `browser-use`/`stagehand`/`playwright-mcp` lanes |
| `BloopAI/vibe-kanban` | 2 | scoped control-plane watch | Paperclip-like control-plane/session UX for coding agents; activity paused since 2026-04-24 in parent pass | high | local app likely | 2026-05-27 19:35 UTC | comment-first | first validate contribution surface and maintainer rhythm; then narrow issue gate |
| `QwenLM/qwen-code` | 2/3 | verify-first | terminal coding-agent runtime bugs | very high | local tests likely | 2026-05-27 17:39 UTC | yes | exact version + duplicate search + language/process gate |
| `gptme/gptme` | 2/3 | verify-first | lower-noise terminal-agent CLI bugs | low | local tests likely | 2026-05-27 17:39 UTC | fixture-only | maintainer rhythm and test harness check |
| `awslabs/cli-agent-orchestrator` | 2/3 | verify-first | multi-agent CLI orchestration | medium | local tests likely | 2026-05-27 17:39 UTC | comment-first | contribution docs + maintainer responsiveness |
| `Open-ACP/OpenACP` | 2/3 | verify-first | ACP bridge/session protocol | low | local tests likely | 2026-05-27 17:39 UTC | comment-first | evidence surface grows beyond early ecosystem watch |
| `microsoft/Webwright` | 3 | scope/watch | browser-agent framework reference | low | browser runtime | 2026-05-27 17:39 UTC | yes | contribution docs/release maturity verified |
| `ServiceNow/BrowserGym` | 3 | scope/watch | web-agent benchmark environment | medium | browser/runtime env | 2026-05-27 17:39 UTC | yes | maintainer responsiveness + direct eval repro |
| `dagger/dagger` | 3 | scope/watch | agent-runner execution graph only | medium | container runtime | 2026-05-27 17:39 UTC | yes | direct Paperclip runner breakage |
| `devcontainers/cli` | 3 | scope/watch | workspace/devcontainer CLI only | high | container runtime | 2026-05-27 17:39 UTC | yes | direct Paperclip workspace failure |

## Cycle 29 overlay

These rows are repo-level decisions from the 3-cycle universe block. They do not authorize upstream comments or PRs.

| Repo | Tier | Next action | Scan lane | Duplicate risk | Runner need | Last checked | avoid_pr_hunting | Promotion gate |
|---|---|---|---|---|---|---|---|---|
| `openai/openai-agents-python` | 1A | active unit-test lane | SDK runtime, tool-call, handoff, tracing regressions | high | local tests first; realtime only with secret-safe repro | 2026-05-27 19:35 UTC | fixture-only | current-main repro + duplicate/competing-PR gate + regression card |
| `pydantic/pydantic-ai` | 1A | active provider/tool/MCP lane | provider parity, tool behavior, MCP integration, usage/accounting | medium-high | local/runner after issue candidate | 2026-05-27 19:35 UTC | fixture-only | issue-level duplicate sweep + regression-first test |
| `BloopAI/vibe-kanban` | 2 | scoped control-plane watch | coding-agent control-plane/session UX | high | local app likely | 2026-05-27 19:35 UTC | comment-first | contribution docs + maintainer rhythm before bug scouting |
| `Infisical/agent-vault` | 3 | create repo-card / infra-adjacent watch | agent credential proxy/vault and secret-boundary infra | medium | local tests likely; avoid private secrets | 2026-05-27 19:35 UTC | comment-first | license text check because GitHub API reports `NOASSERTION`; secret-free repro only |
| `langchain-ai/deepagentsjs` | 3 | verify-first | JS sibling of DeepAgents, skills/tooling/provider propagation | medium | local tests likely | 2026-05-27 19:35 UTC | fixture-only | maturity, issue quality and duplicate gate before promotion |
| `HKUDS/OpenHarness` | 3 | verify-first harness watch | agent harness/evaluation | medium | local tests likely | 2026-05-27 19:35 UTC | fixture-only | young API; promote only after reproducible harness failure |
| `openclaw/openclaw` | 3 | demote / identity-risk | OpenClaude-like repo identity check | unknown | none until provenance verified | 2026-05-27 19:35 UTC | yes | verify issue quality, maintainer loop, and provenance before any repro |
| `Gitlawb/openclaude` | 3 | demote / verify-only | OpenClaude-like repo identity check | unknown | none until canonical status verified | 2026-05-27 19:35 UTC | yes | no PR lanes without external canonical validation |
| `morph-labs/morph-python-sdk` | 3 | close stale generic scope | Morph-specific SDK only | low | cloud account likely | 2026-05-27 19:35 UTC | yes | keep only if Morph-specific target is requested |
| `morph-labs/morph-typescript-sdk` | 3 | close stale generic scope | Morph-specific SDK only | low | cloud account likely | 2026-05-27 19:35 UTC | yes | keep only if Morph-specific target is requested |

## Cycle 28 overlay

These rows are repo-level cards only. They do not authorize upstream comments or PRs.

| Repo | Tier | Next action | Scan lane | Duplicate risk | Runner need | Last checked | avoid_pr_hunting | Promotion gate |
|---|---|---|---|---|---|---|---|---|
| `ChromeDevTools/chrome-devtools-mcp` | 1A | watch / internal candidate `#2138` | Chrome DevTools MCP browser/session/tool lifecycle regressions; path-validation refactor lane in [#54](https://github.com/serejaris/corp-opensource/issues/54) | medium-high | browser runner for perf/session lanes; local/runner tests for `#2138` | 2026-05-27 18:36 UTC | fixture-only | `#2138` call-site scan + fresh duplicate gate + test card; browser lanes need dedicated runner repro |
| `PrefectHQ/fastmcp` | 1A | watch / assignment-first lanes `#4143/#4124` | MCP client/server/tool framework regressions; [#55](https://github.com/serejaris/corp-opensource/issues/55) and [watch](fastmcp-scouting-2026-05-27.md) track `#4143`, `#4124`, `#4192` | high because closed process-gated PRs and active competing PRs | local/runner MRE for `#4143/#4124`; Windows runner/VM for `#4192` | 2026-05-27 18:40 UTC | fixture-only | current-main MRE + assignment-first comment; no PR without maintainer approval/assignment |
| `ag-ui-protocol/ag-ui` | 2 | internal candidate `#1635` | Agent UI protocol / Mastra adapter stream robustness; [#56](https://github.com/serejaris/corp-opensource/issues/56) and [watch](ag-ui-scouting-2026-05-27.md) track payload-less Mastra chunk abort | high repo churn, but exact duplicate not found; related `#1253/#1727/#836` require recheck | runner for pnpm/Nx Mastra unit repro | 2026-05-27 19:10 UTC | comment-first | runner fail-first repro + fresh duplicate gate + assignment/approval before PR |
| `mcp-use/mcp-use` | 2 | watch / duplicate-heavy | MCP app/client/server integration framework; [#57](https://github.com/serejaris/corp-opensource/issues/57) and [watch](mcp-use-scouting-2026-05-27.md) track issue-level duplicate map | high because most narrow lanes are covered by active/merged PRs | runner only after a PR-ready gap appears | 2026-05-27 19:25 UTC | comment-first | watch `#1582/#1578/#1573/#1574/#1572/#1531/#1439/#1455/#1421`; re-open only after unmerged gap, maintainer ask, or current-branch repro |
| `microsoft/agent-framework` | 2/3 | watch | enterprise multi-agent Python/.NET framework | high | local tests likely | 2026-05-27 18:14 UTC | comment-first | exact package/subsystem gate due 676 issues / 268 PRs |
| `langfuse/langfuse` | 2/3 | watch | LLM tracing/eval/observability integrations | high | service stack likely | 2026-05-27 18:14 UTC | comment-first | agent-trace/eval-runner lane only, not generic product support |
| `comet-ml/opik` | 2/3 | watch | tracing/eval/observability for agents | medium | local tests first | 2026-05-27 18:14 UTC | comment-first | scoped SDK/integration/eval-runner bug class |
| `mastra-ai/mastra` | 2 | internal candidate `#17118` | TypeScript agent framework/workflows plus Mastracode TUI; [#58](https://github.com/serejaris/corp-opensource/issues/58) and [watch](mastra-scouting-2026-05-27.md) track slash-command picker wrapping | high repo churn, but no direct PR for `#17118`; many side lanes duplicate-covered | local/runner unit repro for Mastracode component path before PR | 2026-05-27 19:45 UTC | fixture-only | fail-before/pass-after regression card + fresh duplicate gate before any upstream PR |
| `confident-ai/deepeval` | 2/3 | watch | LLM eval framework | medium | local tests likely | 2026-05-27 18:14 UTC | fixture-only | agent/eval harness relevance must be direct |
| `langchain-ai/openevals` | 2/3 | low-touch watch | small eval library/harness | low | local tests likely | 2026-05-27 18:14 UTC | fixture-only | smaller repo; verify maintainer rhythm before promotion |
| `huggingface/smolagents` | 2/3 | watch | lightweight Python agent framework | high | local tests likely | 2026-05-27 18:14 UTC | comment-first | crowded queue; no promotion without concrete regression card |
| `microsoft/magentic-ui` | 3 | watch | experimental browser/local-file agent UI; parent recheck shows `0` open issues / `28` open PRs, so issue intake may be disabled or elsewhere | unknown | browser/runtime likely | 2026-05-27 19:35 UTC | yes | do not treat `0` issues as low backlog; validate intake/contribution surface first |
| `openai/evals` | 3 | demote | archival eval reference | medium | local tests likely | 2026-05-27 18:14 UTC | yes | use newer eval targets first unless direct OpenAI Evals issue appears |
| `anthropics/claude-code` | 3 | no new card | terminal coding-agent reference only | extreme | repo-specific | 2026-05-27 18:14 UTC | yes | only explicitly requested ultra-narrow repro lanes |
