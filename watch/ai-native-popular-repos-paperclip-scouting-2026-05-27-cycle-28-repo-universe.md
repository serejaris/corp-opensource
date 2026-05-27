# Popular AI-native / Paperclip-like repo universe cycle 28, 2026-05-27

Live state: parent `gh repo view` pass на 2026-05-27 18:14 UTC.
Tracker: [serejaris/corp-opensource#52](https://github.com/serejaris/corp-opensource/issues/52).

Upstream actions в этом блоке: 0 PR, 0 comments.

Final `next_status`: `WATCH`.

Cycle 28 является repo-level discovery/triage update. Цель была расширить и проредить popular AI-native/coding-agent/MCP/browser/computer-use/sandbox/skills/eval universe для Paperclip-like strategy. Конкретный upstream issue, comment или PR target не выбран; runner/repro здесь N/A, потому что нет issue-level repro candidate.

`open-source-bug-scouting` и `open-source-pr-workflow` skills в текущей среде не установлены. Вместо них использован допустимый repo fallback: 6 discovery/triage subagents, parent live gates через `gh`, затем 3-subagent critique. Один из 6 subagents ушёл в legacy Rails/Paperclip attachment repos; его вывод зафиксирован как scope drift и не влияет на AI-native ranking.

## Cycle 1: Research / Discovery

Six-subagent roles:

- repo-fit;
- bug-signal / repo-health;
- repro-path / tests-docs;
- patchability / contribution surface;
- duplicate-race / churn;
- PR-readiness / process gates.

Broad candidate universe был расширен в пяти lanes:

| Lane | New / refreshed repos |
|---|---|
| MCP / browser runtime | `ChromeDevTools/chrome-devtools-mcp`, `PrefectHQ/fastmcp`, `mcp-use/mcp-use`, existing `microsoft/playwright-mcp`, MCP official cluster. |
| Agent UI / control-plane protocol | `ag-ui-protocol/ag-ui`, existing `BloopAI/vibe-kanban`, `microsoft/magentic-ui`. |
| Agent frameworks | `huggingface/smolagents`, `mastra-ai/mastra`, `microsoft/agent-framework`, `VoltAgent/voltagent`, existing `pydantic/pydantic-ai`, `openai/openai-agents-python`, `langchain-ai/deepagents`. |
| Evals / observability / harness | `langfuse/langfuse`, `comet-ml/opik`, `confident-ai/deepeval`, `langchain-ai/openevals`, `openai/evals`, existing `promptfoo/promptfoo`. |
| CLI / orchestration | `gptme/gptme`, `awslabs/cli-agent-orchestrator`, existing `SWE-agent/mini-swe-agent`, `cline/cline`, `opencode`, `codex`, `goose`. |

## Parent Live Gates

Authoritative parent snapshot uses `gh repo view` GraphQL counts. These are repo-level freshness signals only; they do not prove any bug is actionable.

| Repo | Stars | Pushed | Release | Open issues | Open PRs | Decision |
|---|---:|---|---|---:|---:|---|
| `ChromeDevTools/chrome-devtools-mcp` | 41969 | 2026-05-27 | `chrome-devtools-mcp-v1.1.1` | 65 | 16 | Promote repo-level scouting; create card. |
| `PrefectHQ/fastmcp` | 25344 | 2026-05-27 | `v3.3.1` | 218 | 15 | Promote repo-level scouting; create card. |
| `ag-ui-protocol/ag-ui` | 13878 | 2026-05-27 | `release/2026-05-26` | 173 | 131 | Create card; watch as Agent UI protocol lane. |
| `mcp-use/mcp-use` | 10006 | 2026-05-27 | `python-v1.7.0` | 54 | 42 | Create card; watch MCP app/client framework lane. |
| `microsoft/agent-framework` | 10790 | 2026-05-27 | `python-1.6.0` | 676 | 268 | Watch only; enterprise framework, high backlog. |
| `langfuse/langfuse` | 28067 | 2026-05-27 | `v3.175.0` | 328 | 320 | Watch only; observability/eval lane, high churn. |
| `comet-ml/opik` | 19383 | 2026-05-27 | `2.0.49` | 86 | 75 | Watch only; scoped eval/trace integration lane. |
| `mastra-ai/mastra` | 24411 | 2026-05-27 | `@mastra/core@1.37.0` | 238 | 215 | Watch; TS agent framework with high PR/issue flow. |
| `confident-ai/deepeval` | 15740 | 2026-05-27 | `v4.0.3` | 210 | 62 | Watch; eval-framework lane, not default PR hunting. |
| `langchain-ai/openevals` | 1064 | 2026-05-19 | `openevals-js==0.2.0` | 8 | 1 | Low-touch watch; low duplicate risk but smaller reach. |
| `huggingface/smolagents` | 27548 | 2026-05-26 | `v1.25.0` | 253 | 304 | Watch; significant but crowded framework. |
| `microsoft/magentic-ui` | 9872 | 2026-05-21 | `v0.2.1` | 0 | 28 | Watch; early/research UI-agent surface, intake unclear. |
| `langchain-ai/open_deep_research` | 11509 | 2026-05-26 | none | 35 | 24 | Watch only; use-case repo, not default generic target. |
| `awslabs/cli-agent-orchestrator` | 638 | 2026-05-26 | `v2.1.1` | 23 | 10 | Keep verify-first CLI orchestration watch. |
| `VoltAgent/voltagent` | 9187 | 2026-05-25 | `@voltagent/server-core@2.1.17` | 29 | 23 | Watch; secondary TS agent platform. |
| `gptme/gptme` | 4310 | 2026-05-27 | `v0.31.0` | 11 | 1 | Passive watch; good local surface but lower leverage. |
| `openai/evals` | 18548 | 2026-04-14 | none | 125 | 80 | Demote to archival eval watch. |
| `anthropics/claude-code` | 126982 | 2026-05-27 | `v2.1.152` | 8813 | 592 | No fresh repo-card; watch only due extreme duplicate/churn risk. |

## Cycle 2: Triage / Fit

### Promote / Create Repo-card

| Repo | Paperclip layer | Operating mode |
|---|---|---|
| `ChromeDevTools/chrome-devtools-mcp` | browser automation, MCP runtime, debugging surface | Active repo-level scouting for tiny browser/MCP regressions; issue-level work requires release-note/linked-PR/duplicate gates. |
| `PrefectHQ/fastmcp` | MCP protocol/runtime/framework | Active repo-level scouting for local fixture bugs in client/server/tool behavior; avoid spec-design PRs without maintainer signal. |
| `ag-ui-protocol/ag-ui` | agent UI / control-plane protocol | Watch Agent UI protocol drift, SDK conformance and examples; no issue-level PR without concrete failure mode. |
| `mcp-use/mcp-use` | MCP apps/client/server integration | Watch integration/runtime bugs with secret-free repro; non-official status means compare against official MCP SDK behavior first. |

### Watch / Scoped Repo-card

| Repo | Mode |
|---|---|
| `microsoft/agent-framework` | Enterprise multi-agent framework; high backlog means comment-first / exact subsystem gates only. |
| `mastra-ai/mastra` | TS agent framework; useful but noisy, require subsystem lane before promotion. |
| `langfuse/langfuse`, `comet-ml/opik`, `confident-ai/deepeval`, `langchain-ai/openevals` | Evals/observability lane. Use only for agent-trace/eval-runner bugs, not generic product support. |
| `huggingface/smolagents`, `microsoft/magentic-ui`, `VoltAgent/voltagent`, `langchain-ai/open_deep_research` | Strategic watch; not default PR hunting. |
| `awslabs/cli-agent-orchestrator` | Keep verify-first control-plane/CLI watch. |

### Demote / No New Card

| Repo | Reason |
|---|---|
| `openai/evals` | Historical eval reference, but weaker current actionability than `openevals`, `deepeval`, `opik`, `langfuse`, and `promptfoo`. |
| `gptme/gptme` | Good local terminal-agent surface, but lower repo-level leverage; keep passive watch until a specific bug class appears. |
| `anthropics/claude-code` | Huge canonical terminal-agent repo, but 8813 open issues / 592 open PRs make duplicate-race extreme. No fresh scouting unless a very specific repro lane is requested. |

## Cycle 3: Critique / Decision

3-subagent critique:

- factology/duplicates: repo-level stars, release and open counts are a snapshot only. Do not infer actionable bugs, duplicate-clean state, maintainer willingness, or `COMMENT-FIRST`/`PR-READY` from these facts. `microsoft/magentic-ui` having `0` open issues may mean disabled or external intake, not clean backlog.
- process gates: cycle is valid only as repo-level AI-native discovery/triage. Upstream comment/PR is not allowed. Legacy Rails/Paperclip attachment output from one subagent is scope drift and excluded from ranking.
- actionability: choose exactly `WATCH` for this block. Repo-level actions are allowed: promote/create repo-card/update scope/demote. Issue-level candidate promotion requires a separate target, parent live gates, runner/repro if needed, regression card, and fresh 3-subagent critique.

Final repo-level actions:

| Action | Repo(s) | Reason |
|---|---|---|
| promote + create repo-card | `ChromeDevTools/chrome-devtools-mcp`, `PrefectHQ/fastmcp` | Strong direct MCP/browser/runtime fit with manageable PR queues relative to bigger MCP repos. |
| create repo-card / active watch | `ag-ui-protocol/ag-ui`, `mcp-use/mcp-use` | Strong AI-native protocol/integration lanes, but higher churn or non-official status. |
| update scope watch lanes | `microsoft/agent-framework`, `mastra-ai/mastra`, `langfuse/langfuse`, `comet-ml/opik`, `confident-ai/deepeval`, `langchain-ai/openevals` | Useful universe expansion, but require narrower subsystem gates. |
| demote/passive watch | `openai/evals`, `gptme/gptme`, `anthropics/claude-code` | Lower immediate actionability or extreme duplicate-race. |

## Next Bounded Cycle

Start with one of:

1. `ChromeDevTools/chrome-devtools-mcp`: narrow issue/PR search around browser context/session/tool lifecycle and Chrome DevTools MCP regressions.
2. `PrefectHQ/fastmcp`: local fixture-friendly MCP client/server/tool behavior scan.
3. `ag-ui-protocol/ag-ui` or `mcp-use/mcp-use`: only after choosing one concrete subsystem and repeating duplicate gates.

No upstream comment or PR until a separate issue-level cycle records issue state, labels/assignee, linked PRs/timeline, duplicate PR search, contribution docs, regression card, repro gate, and 3-subagent critique.
