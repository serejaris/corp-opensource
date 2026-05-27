# Repo cards: AI-native universe, 2026-05-27

Source: [cycle 23 repo-universe update](ai-native-popular-repos-paperclip-scouting-2026-05-27-cycle-23-repo-universe.md).

Purpose: make the next hourly repo-scouting block concrete. These are repo-level cards, not bug candidates.

| Repo | Tier | Next action | Scan lane | Duplicate risk | Runner need | Last checked | Promotion gate |
|---|---|---|---|---|---|---|---|
| `pydantic/pydantic-ai` | 1A | promote | recent provider/tool/MCP regressions with test surface | medium-high | local/runner only after issue candidate | 2026-05-27 17:20 UTC | issue-level duplicate sweep + regression card |
| `OpenHands/OpenHands` | 1A | promote | skills/workspace/sandbox issues; maintainer-direction lanes | high | Docker/runner likely for repro | 2026-05-27 17:20 UTC | comment-first if roadmap ambiguous |
| `e2b-dev/E2B` | 1A | promote | SDK/session/docs bugs with low cloud dependency | low-medium | runner only for live sandbox/session repro | 2026-05-27 17:20 UTC | narrow repro not requiring private secrets |
| `cline/cline` | 1A | promote | deterministic extension/CLI/MCP/provider bugs | high | local checkout, possible extension harness | 2026-05-27 17:20 UTC | Linear/linked PR/duplicate gate clean |
| `trycua/cua` | 1A | promote | computer-use driver/replay/benchmark issues | high | native/runner required for platform bugs | 2026-05-27 17:24 UTC | current platform repro before PR |
| `microsoft/playwright-mcp` | 1A | promote | protocol/tool behavior regressions with tiny fixture | medium-low | browser runtime likely | 2026-05-27 17:20 UTC | issue-first or minimal repro with maintainer-aligned scope |
| `langchain-ai/deepagents` | 1A | promote | filesystem/subagent/tool-call harness regressions | medium | local tests likely enough | 2026-05-27 17:20 UTC | assignment/maintainer gate clean |
| `openai/codex` | 1B | watch | churn, linked PRs, duplicate risk only | extreme | repo-specific | 2026-05-27 17:24 UTC | maintainer signal + ultra-narrow repro |
| `anomalyco/opencode` | 1B | watch | churn, existing PR lane interaction, duplicate risk | extreme | local tests likely, but avoid piling on | 2026-05-27 17:24 UTC | no new candidate promotion this cycle unless distinct stable signal |
| `google-gemini/gemini-cli` | 1B | watch | corporate/high-churn terminal-agent issues | high | local tests likely | 2026-05-27 17:20 UTC | maintainer ack + no competing PR |
| `daytonaio/daytona` | 1B | watch | runner/session lifecycle, no broad infra PRs | high | runner likely | 2026-05-27 17:24 UTC | sharply bounded repro + duplicate gate |
| `browser-use/browser-use` | 2 | update scope | comment-first eligible only for low-duplicate browser regressions | high | browser runtime | 2026-05-27 17:20 UTC | no speculative PR |
| `browserbase/stagehand` | 2 | update scope | watch SDK act/extract/observe regressions | high | browser runtime | 2026-05-27 17:20 UTC | maintainer confirmation before broad SDK PR |
| `PrefectHQ/fastmcp` | 2 | update scope | user-facing MCP framework bugs | medium-high | local tests likely | 2026-05-27 17:20 UTC | avoid spec-design PRs |
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
