# Popular AI-native / Paperclip-like repo universe cycle 29, 2026-05-27

Live state: parent `gh api graphql` pass на 2026-05-27 19:10-19:35 UTC.
Tracker: [serejaris/corp-opensource#52](https://github.com/serejaris/corp-opensource/issues/52).

Upstream actions в этом блоке: 0 PR, 0 comments.

Final `next_status`: `WATCH`.

Это repo-level discovery/triage update, не выбор отдельного bug/issue/PR target. Bugs/issues/PRs ниже использованы только как evidence о repo health, contribution surface и churn risk. Runner repro не запускался: цикл repo-universe не выбрал конкретный candidate issue. `PR-READY` не заявляется.

Required skills `open-source-bug-scouting` и `open-source-pr-workflow` в текущем session skills list недоступны. Использован documented fallback: 6 subagent discovery/triage roles, parent live gates через GitHub GraphQL, затем 3-subagent critique.

## Cycle 1: Research / Discovery

Six-subagent roles были разложены по слоям Paperclip-like strategy:

- terminal coding agent + control-plane;
- browser automation + computer-use + sandbox/session orchestration;
- MCP protocol/runtime/ecosystem;
- skills/capabilities + agent frameworks + provider/tool surfaces;
- eval/harness/observability;
- repo identity / borderline AI-native targets.

Broad candidate universe:

| Layer | Repos refreshed |
|---|---|
| Terminal coding agent | `openai/codex`, `anomalyco/opencode`, `cline/cline`, `aaif-goose/goose`, `google-gemini/gemini-cli`, `QwenLM/qwen-code`, `Aider-AI/aider`, `continuedev/continue`, `BloopAI/vibe-kanban`. |
| Browser / computer-use | `browser-use/browser-use`, `browserbase/stagehand`, `microsoft/playwright-mcp`, `ChromeDevTools/chrome-devtools-mcp`, `trycua/cua`, `Skyvern-AI/skyvern`, `steel-dev/steel-browser`, `ServiceNow/BrowserGym`. |
| Sandbox / session orchestration | `e2b-dev/E2B`, `daytonaio/daytona`, `Infisical/agent-vault`. |
| MCP runtime / discovery | `modelcontextprotocol/python-sdk`, `modelcontextprotocol/typescript-sdk`, `modelcontextprotocol/servers`, `modelcontextprotocol/inspector`, `modelcontextprotocol/registry`, `PrefectHQ/fastmcp`, `mcp-use/mcp-use`, Docker MCP repos, `Open-ACP/OpenACP`. |
| Skills / framework / provider surfaces | `pydantic/pydantic-ai`, `openai/openai-agents-python`, `google/adk-python`, `langchain-ai/deepagents`, `langchain-ai/deepagentsjs`, `ComposioHQ/composio`, `mastra-ai/mastra`, `CopilotKit/CopilotKit`, `huggingface/smolagents`, `crewAIInc/crewAI`, `microsoft/agent-framework`. |
| Eval / harness / observability | `SWE-agent/mini-swe-agent`, `SWE-agent/SWE-agent`, `promptfoo/promptfoo`, `langchain-ai/openevals`, `HKUDS/OpenHarness`, `confident-ai/deepeval`, `comet-ml/opik`, `langfuse/langfuse`, `openai/evals`, `dagger/dagger`, `devcontainers/cli`. |

## Parent Live Gates

Counts are snapshot signals only. They do not prove a repo is low competition, duplicate-clean, or likely to accept a PR.

| Repo | Stars | Pushed | Open issues | Open PRs | Release | Repo-level decision |
|---|---:|---|---:|---:|---|---|
| `anomalyco/opencode` | 166109 | 2026-05-27 | 5268 | 938 | `v1.15.11` | WATCH: very active but saturated; strict duplicate gates only. |
| `google-gemini/gemini-cli` | 104658 | 2026-05-27 | 1198 | 300 | `v0.43.0` | WATCH: strategic terminal-agent reference, comment-first by default. |
| `openai/codex` | 86294 | 2026-05-27 | 4951 | 302 | `rust-v0.134.0` | WATCH: canonical but extreme issue noise. |
| `OpenHands/OpenHands` | 75055 | 2026-05-27 | 164 | 203 | `1.7.0` | WATCH: strong control-plane fit, not low-contention. |
| `cline/cline` | 62410 | 2026-05-27 | 492 | 472 | `v3.85.0` | WATCH: high PR contention relative to issues. |
| `ChromeDevTools/chrome-devtools-mcp` | 41973 | 2026-05-27 | 65 | 16 | `chrome-devtools-mcp-v1.1.1` | PROMOTE: targeted browser/MCP triage surface. |
| `microsoft/playwright-mcp` | 33101 | 2026-05-23 | 6 | 4 | `v0.0.75` | WATCH: small current backlog, but not an easy-PR claim. |
| `browser-use/browser-use` | 95860 | 2026-05-26 | 66 | 179 | `0.12.9` | WATCH: high visibility / high PR pressure. |
| `browserbase/stagehand` | 22826 | 2026-05-27 | 91 | 136 | `stagehand-server-v3/v3.6.10` | WATCH: browser SDK lane, maintainer confirmation before broad PRs. |
| `Skyvern-AI/skyvern` | 21755 | 2026-05-27 | 34 | 123 | `v1.0.36` | WATCH: browser/RPA lane with high PR pressure. |
| `trycua/cua` | 17141 | 2026-05-27 | 128 | 209 | `cua-driver-v0.2.0` | WATCH: computer-use fit, platform repro required. |
| `e2b-dev/E2B` | 12375 | 2026-05-27 | 27 | 35 | `e2b@2.26.0` | WATCH: compact backlog, but active-work checks remain required. |
| `PrefectHQ/fastmcp` | 25346 | 2026-05-27 | 218 | 15 | `v3.3.1` | PROMOTE: MCP framework lane, assignment-first and no spec-design PRs. |
| `modelcontextprotocol/registry` | 6866 | 2026-05-25 | 80 | 16 | `v1.7.9` | WATCH: canonical discovery/trust lane. |
| `modelcontextprotocol/inspector` | 9893 | 2026-05-27 | 168 | 104 | `0.21.2-hotfix-3` | WATCH: MCP devloop UI, affected-area search required. |
| `modelcontextprotocol/python-sdk` | 23152 | 2026-05-27 | 251 | 241 | `v1.27.1` | WATCH: busy official SDK, maintainer-label and linked-PR checks first. |
| `modelcontextprotocol/typescript-sdk` | 12543 | 2026-05-27 | 222 | 208 | `v1.29.0` | WATCH: busy official SDK, duplicate-heavy. |
| `openai/openai-agents-python` | 26705 | 2026-05-26 | 45 | 68 | `v0.17.4` | PROMOTE: high-value unit-test lane, no realtime work without secret-safe repro. |
| `pydantic/pydantic-ai` | 17339 | 2026-05-27 | 384 | 181 | `v1.103.0` | PROMOTE: provider/tool/MCP regressions only. |
| `langchain-ai/deepagents` | 23434 | 2026-05-27 | 134 | 46 | `deepagents==0.6.4` | WATCH: strong framework fit, strict duplicate gate. |
| `langchain-ai/deepagentsjs` | 1270 | 2026-05-22 | 31 | 33 | `deepagents@1.10.2` | WATCH: smaller JS skills/tooling lane; verify maturity before promotion. |
| `ComposioHQ/composio` | 28484 | 2026-05-27 | 28 | 83 | `@composio/claude-agent-sdk@0.9.2` | WATCH: package/path-specific provider/tool lane. |
| `promptfoo/promptfoo` | 21660 | 2026-05-27 | 75 | 245 | `code-scan-action-0.1.6` | WATCH: strong eval fit, high PR collision risk. |
| `SWE-agent/mini-swe-agent` | 4617 | 2026-05-25 | 17 | 9 | `v2.3.0` | PROMOTE: compact harness lane, small enough for full issue sweep. |
| `SWE-agent/SWE-agent` | 19340 | 2026-05-25 | 28 | 26 | `v1.1.0` | WATCH: benchmark runner / Docker-env lane. |
| `BloopAI/vibe-kanban` | 26549 | 2026-04-24 | 376 | 151 | `v0.1.44-20260424091429` | PROMOTE to scoped control-plane watch; first gate is maintainer rhythm. |
| `HKUDS/OpenHarness` | 13194 | 2026-05-27 | 11 | 14 | `v0.1.9` | WATCH: promising harness, young API. |
| `Infisical/agent-vault` | 1443 | 2026-05-26 | 4 | 10 | `v0.22.0` | CREATE CARD: credential proxy/vault lane; license text gate before PR. |
| `microsoft/magentic-ui` | 9872 | 2026-05-21 | 0 | 28 | `v0.2.1` | WATCH: issue intake may be disabled/elsewhere; do not treat `0` as low backlog. |
| `openai/evals` | 18548 | 2026-04-14 | 125 | 80 | none | DEMOTE: archival eval reference. |

## Cycle 2: Triage / Fit

### Promote / Update Scope

| Repo | Paperclip layer | Operating mode |
|---|---|---|
| `ChromeDevTools/chrome-devtools-mcp` | browser automation, MCP runtime | Tier 1A targeted triage for session/tool lifecycle and fixture-friendly regressions; browser lanes need runner-backed repro. |
| `PrefectHQ/fastmcp` | MCP runtime/framework | Tier 1A active watch; assignment-first, no spec-design PR without maintainer signal. |
| `openai/openai-agents-python` | skills/capabilities, SDK runtime, tracing | High-value unit-test lane for tool-call/runtime/handoff; no realtime work without secret-safe repro. |
| `pydantic/pydantic-ai` | skills/tools/providers/MCP | Core active scouting; provider/tool/MCP regressions only, with regression-first tests. |
| `SWE-agent/mini-swe-agent` | terminal coding agent, eval/harness | Compact harness lane; runner repro and duplicate search before any upstream action. |
| `BloopAI/vibe-kanban` | control-plane/session orchestration | Scoped control-plane watch; validate contribution surface and maintainer rhythm before issue-level work. |
| `ag-ui-protocol/ag-ui` | control-plane protocol / agent UI | Active watch only for protocol conformance; high churn, comment-first. |
| `Infisical/agent-vault` | sandbox/session security, credential proxy | Create repo-card as infra-adjacent watch; license text and secret-boundary gates before PR. |

### Watch

| Repo | Mode |
|---|---|
| `mcp-use/mcp-use` | Duplicate-heavy MCP app/client framework; wait for unmerged gap, maintainer ask, or current-branch repro. |
| `microsoft/playwright-mcp` | Strong browser MCP target; current lanes require maintainer direction and browser/CDP runner repro. |
| `OpenHands/OpenHands` | Core fit, but roadmap ambiguity and PR pressure mean comment-first for skills/workspace lanes. |
| `e2b-dev/E2B`, `daytonaio/daytona` | Sandbox/session orchestration; live-cloud and secret boundaries must be handled in runner. |
| `modelcontextprotocol/registry`, `modelcontextprotocol/inspector` | Strategic MCP discovery/debug lanes; separate from SDK/spec bugs. |
| `promptfoo/promptfoo`, `comet-ml/opik`, `langfuse/langfuse`, `confident-ai/deepeval`, `langchain-ai/openevals`, `HKUDS/OpenHarness` | Eval/observability/harness lane; use only scoped SDK/integration/eval-runner bugs. |
| `browser-use/browser-use`, `browserbase/stagehand`, `Skyvern-AI/skyvern`, `trycua/cua` | Browser/computer-use lane; high PR pressure or platform repro requirements. |
| `google-gemini/gemini-cli`, `QwenLM/qwen-code`, `aaif-goose/goose`, `openai/codex`, `anomalyco/opencode`, `cline/cline` | Terminal-agent mega-repos; use only narrow, current-main repros with strict duplicate gates. |

### Demote / Reference Only

| Repo | Reason |
|---|---|
| `openai/evals` | Historical eval reference; newer eval/harness repos have better current actionability. |
| `anthropics/claude-code` | Canonical but extreme duplicate/churn; no fresh scouting without explicit ultra-narrow repro lane. |
| `Aider-AI/aider`, `microsoft/autogen` | Opportunistic only; lower immediate Paperclip payoff or high backlog. |
| `seleniumbase/SeleniumBase`, `ServiceNow/BrowserGym` | Useful browser/repro substrates, not default AI-native upstream targets. |
| `docker/mcp-gateway`, `docker/mcp-registry` | Exact validation/config/catalog bugs only; not broad MCP runtime targets. |
| `morph-labs/morph-python-sdk`, `morph-labs/morph-typescript-sdk` | Close stale generic tracker scope unless a Morph-specific target is requested. |
| `openclaw/openclaw`, `Gitlawb/openclaude` | Identity/provenance risk; verify-only until issue quality and maintainer loop are proven. |

## Cycle 3: Critique / Decision

3-subagent critique:

- fact-check: parent facts are only preliminary scouting synthesis. Avoid claims like "low competition", "best target", or "high stars imply high impact". For repos where PR count is close to or above issue count, PR pressure is non-trivial and requires PR search/timeline checks before upgrade.
- process gates: repo-level final status is `WATCH`. No `PR-READY`, `COMMENT-FIRST`, or `PR-OPEN` status is valid for this block because no concrete upstream issue/repro was selected.
- actionability: update tracker/watch/scope only. The next heartbeat should start from repo-cards/scope, not from an upstream PR.

Final repo-level actions:

| Action | Repo(s) | Reason |
|---|---|---|
| promote / update active scope | `ChromeDevTools/chrome-devtools-mcp`, `PrefectHQ/fastmcp`, `openai/openai-agents-python`, `pydantic/pydantic-ai`, `SWE-agent/mini-swe-agent`, `BloopAI/vibe-kanban`, `ag-ui-protocol/ag-ui` | Strong Paperclip-layer fit, but issue-level gates still required. |
| create repo-card | `Infisical/agent-vault` | Agent credential proxy/vault is infra-adjacent; keep license/secret gates explicit. |
| keep watch | `mcp-use/mcp-use`, `microsoft/playwright-mcp`, `OpenHands/OpenHands`, `e2b-dev/E2B`, `modelcontextprotocol/registry`, `modelcontextprotocol/inspector`, eval/observability lane | Useful repo universe, but no selected candidate issue. |
| demote / reference-only | `openai/evals`, `anthropics/claude-code`, Docker MCP repos, Morph SDKs, `openclaw/openclaw`, `Gitlawb/openclaude` | Lower current actionability, duplicate pressure, tiny public surface, or identity risk. |

## Next Bounded Cycle

Start with repo-card/scope update only; no upstream target selected.

Candidate starting points for the next heartbeat:

1. `SWE-agent/mini-swe-agent`: full issue sweep plus runner repro gate.
2. `openai/openai-agents-python`: unit-test-only tool/runtime/handoff scan, excluding realtime without secret-safe repro.
3. `ChromeDevTools/chrome-devtools-mcp` or `PrefectHQ/fastmcp`: repeat per-issue duplicate gates before any issue-level candidate promotion.
4. `BloopAI/vibe-kanban`: contribution docs and maintainer rhythm validation before bug scouting.
