# Repo Scope: AI-native frameworks and harnesses

Last updated: 2026-05-28

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
| `cline/cline` | `62417` stars, pushed `2026-05-27` | Coding-agent IDE/CLI runtime, MCP/tool/provider bugs, checkpoints, browser automation. After [#72](https://github.com/serejaris/corp-opensource/issues/72) / [watch](cline-cua-playwright-mcp-scope-2026-05-27.md): saturated `WATCH`; wait `#11087` review/merge/close before new Cline implementation work. |
| `OpenHands/OpenHands` | `75060` stars, pushed `2026-05-27` | AI-driven development platform, skills, resolver, sandbox, browser, self-hosting, worker lifecycle. After [#71](https://github.com/serejaris/corp-opensource/issues/71) / [watch](pydantic-openhands-e2b-top-tier-2026-05-27.md): `WATCH`, wait maintainer direction on `#14563`. |
| `pydantic/pydantic-ai` | `17344` stars, pushed `2026-05-27` | Typed agent framework, providers/tools/model settings, strong fixture/test culture. After [#71](https://github.com/serejaris/corp-opensource/issues/71) / [watch](pydantic-openhands-e2b-top-tier-2026-05-27.md): top strategic `WATCH`, but current narrow lanes are covered by same-day PR clusters. |
| `trycua/cua` | `17143` stars, pushed `2026-05-27` | Computer-use agent infrastructure, sandboxes, benchmarks, trajectory recording. After [#72](https://github.com/serejaris/corp-opensource/issues/72) / [watch](cline-cua-playwright-mcp-scope-2026-05-27.md): `WATCH`, with `#1739` as lead-after-macOS Retina/AVF repro. |
| `web-infra-dev/midscene` | `13451` stars, pushed `2026-05-28` | UI automation / browser-use / phone-use framework for agent workflows. Added by [cycle 35](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-35-cua-ui-automation.md) and refreshed at `2026-05-28 03:16 UTC` as the second practical runner target after `probe#568`; `#2544` longPress cap has no exact PR cover, but stays `WATCH` until current-main timing evidence proves requested duration is capped/truncated and a package test contract is available. |
| `bytedance/UI-TARS-desktop` | `35528` stars, pushed `2026-05-18` | Desktop GUI-agent / CUA stack with MCP/server surfaces. Added by [cycle 35](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-35-cua-ui-automation.md) as high-value passive `WATCH`; fresh lanes are macOS/device/topmost/iPhone-mirror specific and need dedicated environment evidence before any upstream action. |
| `CursorTouch/Windows-MCP` | `5758` stars, pushed `2026-05-26` | Windows MCP/UI automation server. Added by [cycle 35](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-35-cua-ui-automation.md) as `WATCH / duplicate-sensitive`: `#236` has active external UAC/service work, and `#82/#99` coordinate drift needs Windows DPI repro plus stale PR `#81/#93` applicability check. |
| `langchain-ai/deepagents` | `23435` stars, pushed `2026-05-27` | Agent harness with LangChain maintainership; likely tool/memory/runtime tests. After [#73](https://github.com/serejaris/corp-opensource/issues/73) / [watch](fastmcp-openai-agents-deepagents-scope-2026-05-27.md): `WATCH`, with `#3639` as lead-after-repro. |
| `e2b-dev/E2B` | `12375` stars, pushed `2026-05-27` | Secure sandbox/runtime layer for agents. After [#71](https://github.com/serejaris/corp-opensource/issues/71) / [watch](pydantic-openhands-e2b-top-tier-2026-05-27.md): best pragmatic runner-backed follow-up, with `#1352` as lead-after-repro. |
| `microsoft/playwright-mcp` | `33108` stars, pushed `2026-05-23`; parent count `6` open issues / `4` open PRs on `2026-05-27` | Canonical browser automation MCP server; small backlog and concrete tool/protocol regression surface. After [#72](https://github.com/serejaris/corp-opensource/issues/72) / [watch](cline-cua-playwright-mcp-scope-2026-05-27.md): best immediate follow-up, but `#1635` remains approval/assignment-gated. |
| `ChromeDevTools/chrome-devtools-mcp` | `41969` stars, pushed `2026-05-27`; parent count `65` open issues / `16` open PRs on `2026-05-27` | Chrome DevTools MCP browser/debugging runtime for coding agents; separate lane from Playwright MCP. |
| `PrefectHQ/fastmcp` | `25354` stars, pushed `2026-05-27`; parent count `218` open issues / `15` open PRs on `2026-05-27` | Popular MCP framework/runtime with local fixture-friendly client/server/tool surfaces. After [#73](https://github.com/serejaris/corp-opensource/issues/73) / [watch](fastmcp-openai-agents-deepagents-scope-2026-05-27.md): best MCP/server follow-up, but assignment-first and `#4246` is blocked by maintainer PR `#4248`. |
| `google/adk-python` | Issue `#5864` active on `2026-05-27`; repo has contribution guide, PR template, unit tests, tox, and recent optional-dependency import PRs | Agent Development Kit: agent runtime/framework, code execution, memory, evaluation, Vertex AI integrations. Good for dependency-boundary and harness reliability bugs when repo gates allow. |
| `openai/openai-agents-python` | `26705` stars, pushed `2026-05-26`; parent count `45` open issues / `69` open PRs on `2026-05-27` | Agent SDK/runtime with tools, handoffs, tracing and testable unit-level surfaces. After [#73](https://github.com/serejaris/corp-opensource/issues/73) / [watch](fastmcp-openai-agents-deepagents-scope-2026-05-27.md): strategic `WATCH`, because current narrow lanes are covered by active PRs. |
| `SWE-agent/mini-swe-agent` | `4625` stars, pushed `2026-05-25`; parent count `19` open issues / `9` open PRs on `2026-05-28` | Compact terminal-agent and eval/harness target; active `WATCH` via [#53](https://github.com/serejaris/corp-opensource/issues/53) / [follow-up](mini-swe-agent-826-followup-2026-05-27.md): `#826` still open, current `local.py` still has shell timeout without process-group cleanup, but merged `#832` may partially cover the timeout symptom. No upstream action until post-`#832` runner repro proves a remaining leak. |
| `stablyai/orca` | `3545` stars, pushed `2026-05-28`; parent count `93` open issues / `136` open PRs on `2026-05-28`; latest `v1.4.30` | Coding-agent IDE/control plane with worktrees, remote SSH and file streaming. Internal `WATCH / duplicate-covered` after [#82](https://github.com/serejaris/corp-opensource/issues/82) / [watch](orca-2930-short-stream-zero-fill.md): `#2930` is exact-covered by open upstream PR `#2948`; no runner/upstream action while that PR is live. |

## Tier 1B: Strategic Watch, Not Blind PR Hunting

These repos are strategically important but too noisy for random bug hunting. Use only after strict live gates and a narrow repro.

| Repo | Current live signal | Why watch-first |
|---|---|---|
| `openai/codex` | `86314` stars, pushed `2026-05-27`; API parent count `5264` issues+PRs; `304` open PRs; latest `rust-v0.134.0` | Direct terminal coding-agent reference, but after [#74](https://github.com/serejaris/corp-opensource/issues/74) / [watch](high-churn-terminal-runner-scope-2026-05-27.md) keep evidence-only `WATCH`: external code PRs are invitation-only and fresh sandbox/MCP/plugin lanes need current-main repro plus duplicate/PR search. |
| `anomalyco/opencode` | `166142` stars, pushed `2026-05-27`; API parent count `6219` issues+PRs; `939` open PRs; latest `v1.15.11` | Huge active terminal-agent ecosystem; after [#74](https://github.com/serejaris/corp-opensource/issues/74) / [watch](high-churn-terminal-runner-scope-2026-05-27.md) keep saturated `WATCH`: no new opencode candidate while current PRs await review unless a distinct stable gap has failing test, assignment/process gate and duplicate sweep. |
| `google-gemini/gemini-cli` | `104660` stars, pushed `2026-05-27`; API parent count `1477` issues+PRs; `279` open PRs; latest `v0.44.0` | Official Gemini CLI agent; after [#74](https://github.com/serejaris/corp-opensource/issues/74) / [watch](high-churn-terminal-runner-scope-2026-05-27.md) use approved/help-wanted issue path only, with current-main repro and duplicate-label/linked-PR resolution. |
| `daytonaio/daytona` | `72456` stars, pushed `2026-05-27`; `276` open issues / `130` open PRs; latest `v0.182.0` | Secure dev env / runner/session control-plane; after [#74](https://github.com/serejaris/corp-opensource/issues/74) / [watch](high-churn-terminal-runner-scope-2026-05-27.md) best later runner follow-up, but `#4750/#4805` appear covered and `#4842` is security-sensitive, so promotion needs post-PR-settle gap scan plus runner-backed repro. |
| `modelcontextprotocol/registry` | `6867` stars, pushed `2026-05-25`; `80` open issues / `17` open PRs; latest `v1.7.9` | Canonical MCP discovery/trust/metadata layer. After [#75](https://github.com/serejaris/corp-opensource/issues/75) / [watch](mcp-ecosystem-repo-scope-2026-05-27.md): best later publisher/validator follow-up, but current fresh bugs `#1306/#1289/#1184` are PR-covered; do not PR `data/seed.json` or operational listing/delete changes. |
| `modelcontextprotocol/inspector` | `9894` stars, pushed `2026-05-27`; `170` open issues / `104` open PRs; latest `0.21.2-hotfix-3` | MCP devloop/debug UI/runtime. After [#75](https://github.com/serejaris/corp-opensource/issues/75) / [watch](mcp-ecosystem-repo-scope-2026-05-27.md): strongest MCP tooling fit, but V2 churn and nearby PRs block cold action; use only V1 bug/spec-compliance or maintainer-aligned V2 issue after linked PR search and test card. |
| `modelcontextprotocol/servers` | `86341` stars, pushed `2026-05-27`; `293` open issues / `234` open PRs; latest `2026.1.26` | Canonical server hub. After [#75](https://github.com/serejaris/corp-opensource/issues/75) / [watch](mcp-ecosystem-repo-scope-2026-05-27.md): package-level reference-server watch only; no new server/listing PRs, and current git/filesystem/fetch clusters are heavily PR-covered. |
| `modelcontextprotocol/modelcontextprotocol` | `8242` stars, pushed `2026-05-27`; `130` open issues / `112` open PRs; latest `2025-11-25` | MCP spec/docs reference. After [#75](https://github.com/serejaris/corp-opensource/issues/75) / [watch](mcp-ecosystem-repo-scope-2026-05-27.md): spec/docs/schema watch only after SEP/governance alignment, source/generated validation, and AI disclosure planning for any upstream comment/PR. |
| `aaif-goose/goose` | `45956` stars, pushed `2026-05-27`; parent count `130` open issues / `74` open PRs | Local terminal/tool agent; existing PR-open and comment-first lanes mean watch, not fresh PR hunting. |

## Tier 2: Watch / Comment-First Before Bug Scouting

These can produce good candidates, but the next step must be repo-specific gates and issue/PR search rather than broad PR hunting.

| Repo | Why watch first |
|---|---|
| `browser-use/browser-use` | `WATCH` after [#70](https://github.com/serejaris/corp-opensource/issues/70) / [watch](browser-agent-repo-scope-2026-05-27.md): strong browser-agent fit, but parent count `66` open issues / `179` open PRs and high duplicate race. Best lanes `#4846/#4801/#4824/#4860` overlap active PRs; use post-merge gap scan only. |
| `browserbase/stagehand` | `WATCH` after [#70](https://github.com/serejaris/corp-opensource/issues/70) / [watch](browser-agent-repo-scope-2026-05-27.md): strongest future browser-SDK lane, but active queue and external PR approval flow block broad PR hunting. Wait for gaps after `#2162/#2166/#2156/#2062/#2159/#2048`. |
| `modelcontextprotocol/python-sdk` | Official SDK, but parent count `251` open issues / `241` open PRs and spec churn. |
| `modelcontextprotocol/typescript-sdk` | Official SDK, but parent count `222` open issues / `207` open PRs and duplicate-heavy PR flow. |
| `langchain-ai/langgraph` | Active graph/runtime ecosystem; crowded, fast-moving, only crisp regressions. |
| `CopilotKit/CopilotKit` | Frontend stack/protocol for agents and generative UI; continue existing PR/watch lanes. |
| `continuedev/continue` | IDE/CLI/CI agent workflow; verify product-direction fit before heavy scouting. |
| `crewAIInc/crewAI` | `WATCH` after [#79](https://github.com/serejaris/corp-opensource/issues/79) / [watch](orchestration-tool-eval-framework-scope-2026-05-27.md): popular orchestration framework, but PR queue is saturated (`326` open PRs) and fresh lanes such as `#5930/#5931/#5886` are PR-covered or high-overlap. Comment-first/process gate with `llm-generated` label before any upstream action. |
| `ComposioHQ/composio` | `WATCH` after [#79](https://github.com/serejaris/corp-opensource/issues/79) / [watch](orchestration-tool-eval-framework-scope-2026-05-27.md): tool registry/auth/integration monorepo; package/path-specific only because default branch is `next`, contribution text mentions ISC, and fresh MCP/API/auth failures are hosted-service dependent without managed-secret runner repro. |
| `promptfoo/promptfoo` | `WATCH` after [#79](https://github.com/serejaris/corp-opensource/issues/79) / [watch](orchestration-tool-eval-framework-scope-2026-05-27.md): best future eval/assertion/redteam/tooling deep-dive, but `#9235` is duplicate-covered by `#9239/#9298` and fresh trajectory/callback lanes have active PRs. Promote only after clean duplicate gate and runner-backed current-main repro. |
| `SWE-agent/SWE-agent` | Coding-agent harness and runner; useful for benchmark runner, Docker/env and docs drift bugs. |
| `SWE-agent/mini-swe-agent` | Promoted to Tier 1A after cycle 29; kept here only as a reminder to cross-check with `SWE-agent/SWE-agent` before treating a candidate as independent. |
| `Skyvern-AI/skyvern` | `WATCH` after [#70](https://github.com/serejaris/corp-opensource/issues/70) / [watch](browser-agent-repo-scope-2026-05-27.md): browser workflow/RPA reference; AGPL and service-stack caution. Take only exact self-hosted/current-main bugs with runner evidence, not cloud/account support issues. |
| `BloopAI/vibe-kanban` | Paperclip-like control-plane for coding agents; [bounded scan](vibe-kanban-scouting-2026-05-27.md) kept `WATCH` because repo is sunsetting, README asks for discussion before PR, and maintainer rhythm is weak. `#3329` is lead-after-repro only; no PR hunting. |
| `QwenLM/qwen-code` | `WATCH` after [#68](https://github.com/serejaris/corp-opensource/issues/68) / [watch](qwen-code-agent-framework-scouting-2026-05-27.md): terminal coding agent, but verify-first only. Strongest lanes `#4450/#4466/#4363/#4364` are covered by active PRs or nearby work; promote only after one exact issue, fresh PR search, Node >=22 runner repro and targeted test/preflight card. |
| `gptme/gptme` | Passive `WATCH` after [#69](https://github.com/serejaris/corp-opensource/issues/69) / [watch](gptme-cao-openacp-verify-first-2026-05-27.md): lower-noise terminal agent with good CLI/tooling fit, but current open issues are mostly feature/design or gap-check lanes. Promote only for small current-main CLI/tooling/session regression with targeted `make test` card. |
| `awslabs/cli-agent-orchestrator` | Main verify-first `WATCH` after [#69](https://github.com/serejaris/corp-opensource/issues/69) / [watch](gptme-cao-openacp-verify-first-2026-05-27.md): multi-agent CLI control-plane; best follow-up lanes `#131/#78/#164` around inbox/tmux/status delivery, blocked by overlap with `#115/#251` and runner repro. |
| `Open-ACP/OpenACP` | Ecosystem `WATCH` after [#69](https://github.com/serejaris/corp-opensource/issues/69) / [watch](gptme-cao-openacp-verify-first-2026-05-27.md): ACP bridge/control-plane with low current bug surface; `#247` covered by `#251`, `#240/#232` appear fixed in adapter commits. |
| `ag-ui-protocol/ag-ui` | Agent UI/control-plane protocol; active but high churn, use for SDK/protocol conformance only after subsystem gate. |
| `mcp-use/mcp-use` | MCP app/client/server framework; useful integration surface, but compare against official SDK behavior before calling a bug. |
| `microsoft/agent-framework` | `WATCH` after [#68](https://github.com/serejaris/corp-opensource/issues/68) / [watch](qwen-code-agent-framework-scouting-2026-05-27.md): Microsoft Python/.NET agent framework; high backlog and active internal PR flow mean exact package/subsystem gate only. Best later lanes are `#6120/#6006`; Python Foundry, inline skill args, AG-UI chunks and hosted MCP lanes are already assigned or covered by active PRs. |
| `mastra-ai/mastra` | TypeScript agent framework/workflows; useful watch but high issue/PR flow. |
| `deepset-ai/haystack` | `WATCH` after [#77](https://github.com/serejaris/corp-opensource/issues/77) / [watch](rag-memory-retrieval-repo-scope-2026-05-27.md): strongest RAG pipeline/process fit in this cluster, but fresh evaluator/retriever/metadata lanes are PR-covered or need one uncovered regression niche plus CLA/release-note/runner gates. |
| `run-llama/llama_index` | `WATCH` after [#77](https://github.com/serejaris/corp-opensource/issues/77) / [watch](rag-memory-retrieval-repo-scope-2026-05-27.md): huge RAG/agent surface, but `186` open issues / `216` open PRs and exact `#21750 -> #21785` coverage make it post-PR-settle only. |
| `mem0ai/mem0` | `WATCH` after [#77](https://github.com/serejaris/corp-opensource/issues/77) / [watch](rag-memory-retrieval-repo-scope-2026-05-27.md): strong AI-agent memory fit, but `291` open PRs, sparse labels and active overlap require manual PR search and runner-backed memory repro. |
| `qdrant/qdrant` | `WATCH` after [#77](https://github.com/serejaris/corp-opensource/issues/77) / [watch](rag-memory-retrieval-repo-scope-2026-05-27.md): vector-core reference, but Rust/service-stack and `dev` branch process make it runner-first; `#9201` is covered by `#9202`. |
| `lancedb/lancedb` | `WATCH` after [#77](https://github.com/serejaris/corp-opensource/issues/77) / [watch](rag-memory-retrieval-repo-scope-2026-05-27.md): useful embedded retrieval store and cross-language binding target; promote only for non-covered client/runtime bugs with targeted tests. |
| `FlowiseAI/Flowise` | `WATCH` after [#78](https://github.com/serejaris/corp-opensource/issues/78) / [watch](low-code-agent-workflow-builder-scope-2026-05-27.md): best lightweight visual-agent builder follow-up, but `#6428` is exact-covered by `#6429`; promote only after a distinct API/Agentflow bug with package-level repro. |
| `activepieces/activepieces` | `WATCH` after [#78](https://github.com/serejaris/corp-opensource/issues/78) / [watch](low-code-agent-workflow-builder-scope-2026-05-27.md): good workflow/runtime/MCP control-plane target, but `#13418` is exact-covered by `#13420`; API/runtime repro often needs runner. |
| `langgenius/dify` | `WATCH` after [#78](https://github.com/serejaris/corp-opensource/issues/78) / [watch](low-code-agent-workflow-builder-scope-2026-05-27.md): very strong agentic workflow platform, but high churn, modified license, issue/assignment gate and service-stack runner needs block cold action. |
| `langflow-ai/langflow` | `WATCH` after [#78](https://github.com/serejaris/corp-opensource/issues/78) / [watch](low-code-agent-workflow-builder-scope-2026-05-27.md): strong visual agent/workflow fit, but huge PR backlog and release-branch/security gates require exact narrow non-overlap. |
| `n8n-io/n8n` | `WATCH` after [#78](https://github.com/serejaris/corp-opensource/issues/78) / [watch](low-code-agent-workflow-builder-scope-2026-05-27.md): mature workflow automation with AI/MCP lanes, but Linear/team-owned triage, CLA, Sustainable Use/EE boundaries and 1000+ PRs make it maintainer-alignment only. |
| `langfuse/langfuse` | `WATCH` after [#76](https://github.com/serejaris/corp-opensource/issues/76) / [watch](observability-eval-harness-repo-scope-2026-05-27.md): strongest observability/eval/MCP fit, but parent count `328` open issues / `320` open PRs and same-day MCP/API/evals PR churn block cold action. Promote only after one exact issue, linked PR search, core-vs-SDK/EE gate and runner/service repro. |
| `comet-ml/opik` | `WATCH` after [#76](https://github.com/serejaris/corp-opensource/issues/76) / [watch](observability-eval-harness-repo-scope-2026-05-27.md): strong tracing/online-eval/SDK platform, but CLA, tracked issue/Jira, AI-WATERMARK and internal PR stream require issue-specific gates. |
| `confident-ai/deepeval` | `WATCH` after [#76](https://github.com/serejaris/corp-opensource/issues/76) / [watch](observability-eval-harness-repo-scope-2026-05-27.md): relevant eval framework; current best metric lanes are PR-covered, so future promotion needs a direct eval-harness current-main failing fixture. |
| `langchain-ai/openevals` | Low-touch `WATCH` after [#76](https://github.com/serejaris/corp-opensource/issues/76) / [watch](observability-eval-harness-repo-scope-2026-05-27.md): small LangChain eval harness with low visible duplicate risk but mostly old feature/API-shape issues. |
| `huggingface/smolagents` | `WATCH` after [#76](https://github.com/serejaris/corp-opensource/issues/76) / [watch](observability-eval-harness-repo-scope-2026-05-27.md): lightweight agent framework with good runner-light executor/tool bug potential, but `253` open issues / `304` open PRs and security/runtime churn require exact duplicate search and current-main repro. |
| `microsoft/magentic-ui` | Experimental browser/local-file agent UI; intake and contribution surface need validation. |
| `vercel/ai` | High-value AI SDK watch after [#66](https://github.com/serejaris/corp-opensource/issues/66) / [watch](ai-sdk-agents-repo-scope-2026-05-27.md): exact provider/schema/tool-call/streaming validation bugs only. Very duplicate-heavy; promote only after targeted PR search and fixture/unit repro in a Node/pnpm runner. |
| `probelabs/probe` | Primary runner `CANDIDATE` after [#80](https://github.com/serejaris/corp-opensource/issues/80), [watch](coding-assistant-context-code-search-scope-2026-05-27.md), [02:19 refresh](candidate-backlog-refresh-2026-05-28-0219.md), and [03:16 follow-up](upstream-followup-2026-05-28.md): compact semantic code-search/MCP target. Selected lane `#568` needs runner fail-before for `symbols` JSON stdout pollution before any upstream comment/PR; exact root cause is still unproven because `symbols.rs` looks JSON-clean and suspicious `Pattern:` / `Path:` output is in `main.rs::handle_search`. Minimal contract: capture stdout/stderr/exit/JSON parse on current main and require machine JSON-only stdout. |
| `Fission-AI/OpenSpec` | `WATCH` after [#80](https://github.com/serejaris/corp-opensource/issues/80) / [watch](coding-assistant-context-code-search-scope-2026-05-27.md): spec-driven development for coding assistants, but current Codex/spec lanes are PR-covered or high-overlap and formal contribution/security docs are weak. |
| `safishamsi/graphify` | `WATCH` after [#80](https://github.com/serejaris/corp-opensource/issues/80) / [watch](coding-assistant-context-code-search-scope-2026-05-27.md): high-fit code/context knowledge graph, but default branch `v8`, huge PR queue and suspicious star-spike/provenance risk block candidate promotion without exact repro and duplicate gate. |
| `TabbyML/tabby` | `WATCH` after [#80](https://github.com/serejaris/corp-opensource/issues/80) / [watch](coding-assistant-context-code-search-scope-2026-05-27.md): mature self-hosted coding assistant, but current lanes are PR-covered/fuzzy and meaningful server/model/private-repo repro often needs Docker/GPU/secrets; avoid `ee/` license surface. |
| `generalaction/emdash` | `WATCH / lead-scout` after [cycle 31](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-31-repo-universe.md): agentic dev environment / multi-agent control plane. Later lanes `#1875` Linux safeStorage and `#2110` PTY `setsid()` cleanup need design/repro gates. |
| `asheshgoplani/agent-deck` | `WATCH / lead-scout` after [cycle 31](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-31-repo-universe.md): compact terminal session manager for Claude/Gemini/OpenCode/Codex. Backup lane `#1212` is runner-friendly; avoid `#1205` while maintainer has queued a fix. |
| `agentgateway/agentgateway` | `WATCH` after [cycle 31](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-31-repo-universe.md): MCP/AI gateway and proxy runtime. Many fresh lanes are exact-covered or process-heavy; use comment-first/good-first only after duplicate gate. |
| `googleapis/mcp-toolbox` | `WATCH / comment-first` after [cycle 31](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-31-repo-universe.md): official MCP database toolbox. Google CLA, issue-first process and integration-test expectations make `#3286` maintainer-guidance-first, not PR-first. |
| `oraios/serena` | Internal `CANDIDATE` after [cycle 32](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-32-repo-universe.md) via [#84](https://github.com/serejaris/corp-opensource/issues/84): semantic retrieval/editing MCP toolkit. Follow-up `2026-05-28 01:46 UTC` closed the duplicate-search caveat for selected lane `#1519`; no covering PR found. Runner repro is still missing, and any fix/comment must preserve dashboard Host validation rather than wildcarding/skip-checking it. |
| `vercel-labs/agent-browser` | `WATCH` after [cycle 32](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-32-repo-universe.md): browser automation CLI for AI agents with very high PR pressure. Many fresh issues already have exact PRs; promote only one uncovered issue after full PR search and runner/toolchain readiness. |
| `colbymchenry/codegraph` | `WATCH / stale-conflicting duplicate-covered` after [cycle 32](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-32-repo-universe.md) and [active PR refresh](upstream-followup-2026-05-28.md): local code graph for coding agents. `#507` Hermes YAML indentation is covered by open exact-fix PR `#345`, but latest live gate reports `#345` as `CONFLICTING/DIRTY` with no checks. No independent action while that PR is live unless it closes, maintainer asks for replacement, or fresh current-main repro proves an uncovered gap. |
| `epiral/bb-browser` | `WATCH` after [cycle 32](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-32-repo-universe.md): Chrome login-state CLI/MCP browser control. `#217` Windows daemon port conflict is plausible future comment-first, but needs Windows/port-policy validation and runner evidence. |
| `agent-infra/sandbox` | `LEAD` after [cycle 32](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-32-repo-universe.md): all-in-one browser/shell/file/MCP sandbox. `#193` timeout behavior may be real, but image/server source boundary is unclear; needs source-boundary evidence and container repro. |
| `browser-use/browser-harness` | `WATCH / duplicate-covered` after [cycle 33](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-33-fresh-discovery.md): browser-agent harness and CDP helper layer. `#382` `press_key` double-types printable characters is high-signal but not a candidate because open PR `#346` implements the exact `keyDown` text removal and `#332/#297` overlap `press_key` and unit tests. Re-enter only if covers settle without fixing current `main`, then rerun duplicate search and Chrome/CDP runner repro. |
| `farion1231/cc-switch` | `WATCH` after [cycle 33](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-33-fresh-discovery.md): high-fit Claude Code/Codex/OpenCode/Hermes control-plane with MCP/provider/session surfaces. Current reasoning/token lanes are duplicate-heavy and PR-saturated, so use post-merge gap scanning only. |
| `esengine/DeepSeek-Reasonix` | `WATCH` after [cycle 33](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-33-fresh-discovery.md): DeepSeek-native terminal/desktop coding agent. Exact covers exist for `#2051` and `#2081`; `#2067` needs fresh current-version repro because related TUI-loop fixes already landed. |
| `OpenCoworkAI/open-cowork` | `WATCH` after [cycle 33](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-33-fresh-discovery.md): desktop AI agent app with Claude Code, MCP, skills, sandbox and multi-model surfaces. Current issues are assigned, reporter-resolved, or too broad for action without platform logs. |
| `affaan-m/ECC` | `WATCH / anomaly-release` after [cycle 33](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-33-fresh-discovery.md): thematically relevant skills/memory/security layer, but provenance/anomaly risk is high. `#2049` is already fixed on `main` / `v2.0.0-rc.1`; current blocker is release publication, not code. |
| `ref-tools/ref-tools-mcp` | `NO-GO bug-hunting / reference WATCH` after [cycle 33](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-33-fresh-discovery.md): MCP docs/reference tool with low-noise value, but current open issues are product/support asks rather than patchable bug lanes. |
| `HKUDS/nanobot` | `WATCH` after [cycle 34](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-34-workflow-agent-platforms.md): high-fit Python agent runtime with active maintainer cadence, providers, MCP/tools and conversation-history surfaces. Current strong lanes `#4006/#4013` are exact-covered by open PRs `#4011/#4020`; re-enter only after those PRs settle or a distinct bug survives duplicate gate. |
| `coasty-ai/open-computer-use` | `WATCH` after [cycle 35](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-35-cua-ui-automation.md): Apache-2 computer-use agent with fresh activity and good thematic fit, but no open issues/PRs at live gate; treat as reference watch until an exact issue appears or code-search finds a runner-proven bug. |
| `the-open-agent/openagent` | Passive `WATCH` after [cycle 35](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-35-cua-ui-automation.md): agent harness with browser/computer-use/coding-agent surfaces, but current actionable lane `#2303` needs Windows package/binary repro while `#2281/#2282` are update/advice paths. |
| `higress-group/himarket` | Ecosystem reference after [cycle 35](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-35-cua-ui-automation.md): AI capability marketplace / MCP registry with license `Other`; weak direct Paperclip-like fit unless a narrow agent/tool registry validation bug survives service-stack repro. |
| `coze-dev/coze-studio` | `LEAD` after [cycle 34](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-34-workflow-agent-platforms.md): large AI agent studio/workflow platform. Best fresh signal is `#2683` Signal Arena / Agent World auth failure, but parent code search for `world.coze.site` returned `0`, so OSS ownership is unproven; promote only after source/config ownership or maintainer confirmation. |
| `triggerdotdev/trigger.dev` | `COMMENT-FIRST / WATCH` after [cycle 34](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-34-workflow-agent-platforms.md): active workflow/agent runtime, but contribution docs require vouch and unvouched PRs are auto-closed. `#3672/#3674` had exact closed PR attempts; `#3726` needs maintainer/vouch signal before any patch. |
| `lastmile-ai/mcp-agent` | `WATCH` after [cycle 34](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-34-workflow-agent-platforms.md): strong MCP agent framework fit, but current best bug `#671` is exact-covered by open PRs `#675/#686` and repo merge cadence is weak. Use only small issue-first lanes after duplicate gate. |
| `PySpur-Dev/pyspur` | `WATCH` after [cycle 34](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-34-workflow-agent-platforms.md): visual agent workflow playground with strong thematic fit but weak acceptance cadence. `#294` is exact-covered by open PR `#302`; broader lanes need freshness/canonical repo checks before candidate promotion. |
| `julep-ai/julep` | `NO-GO bug-hunting / archival WATCH` after [cycle 34](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-34-workflow-agent-platforms.md): serverless agent workflow platform with lifecycle/shutdown signals and cold merge cadence. Avoid security/public-infra issues `#1595/#1602` without official maintainer/security process signal. |

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
| `microsoft/autogen` | `WATCH` after [#79](https://github.com/serejaris/corp-opensource/issues/79) / [watch](orchestration-tool-eval-framework-scope-2026-05-27.md): large Microsoft framework; lower current push freshness, high backlog/triage risk, Microsoft CLA, and current shared-memory/docs lanes already have active PRs. |
| `stanfordnlp/dspy` | `WATCH` after [#79](https://github.com/serejaris/corp-opensource/issues/79) / [watch](orchestration-tool-eval-framework-scope-2026-05-27.md): strong LM programming/adapters/tool-call framework, but current tool/adapters/eval lanes are crowded and CONTRIBUTING rejects fully autonomous AI-agent PRs; needs human-owned disclosure, exact duplicate gate and runner/current-main repro. |
| `seleniumbase/SeleniumBase` | Strong browser automation substrate, but not AI-native by default. |
| `steel-dev/steel-browser` | Relevant smaller browser sandbox; verify maintainer rhythm before promotion. |
| `morph-labs/morph-python-sdk` | `NO-GO` for generic scouting after [#64](https://github.com/serejaris/corp-opensource/issues/64) / [watch](morph-sdk-generic-scope-closure-2026-05-27.md): low public surface, 0 open issues, weak contribution process, cloud/API-key repro risk. Revisit only for Morph-specific request or fresh current-main bug card. |
| `morph-labs/morph-typescript-sdk` | `NO-GO` for generic scouting after [#64](https://github.com/serejaris/corp-opensource/issues/64) / [watch](morph-sdk-generic-scope-closure-2026-05-27.md): low public surface, stale issues with maintainer replies, stale/conflict-heavy PR queue, cloud/API-key repro risk. Revisit only for Morph-specific request or fresh current-main bug card. |
| `docker/mcp-gateway` | `WATCH` after [#65](https://github.com/serejaris/corp-opensource/issues/65) / [watch](docker-mcp-scope-2026-05-27.md): useful MCP distribution/gateway reference, but exact validation/config only. Watch leads such as `#442` and `#430` require one-issue duplicate gate, runner repro and regression card before any promotion. |
| `docker/mcp-registry` | `NO-GO` for generic scouting after [#65](https://github.com/serejaris/corp-opensource/issues/65) / [watch](docker-mcp-scope-2026-05-27.md): registry/catalog queue is noisy and PR-heavy; keep only as reference / exact metadata-validation target by concrete linked card. |
| `cloudflare/agents` | `WATCH` after [#66](https://github.com/serejaris/corp-opensource/issues/66) / [watch](ai-sdk-agents-repo-scope-2026-05-27.md): high-fit agent runtime/MCP/streaming/compaction reference, but upstream README says external PRs are not accepted currently. Use issue-first / maintainer-signal only. |
| `microsoft/Webwright` | Browser-agent framework; reference/watch until contribution docs and release maturity are clearer. |
| `ServiceNow/BrowserGym` | Web-agent benchmark environment; research/benchmark watch, not default PR lane. |
| `dagger/dagger` | Execution graph/container infra; only direct agent-runner breakages belong here. |
| `devcontainers/cli` | Workspace/devcontainer CLI; only direct Paperclip runner/workspace failures belong here. |
| `openai/evals` | Historical eval reference; prefer newer eval targets unless a direct OpenAI Evals bug appears. |
| `anthropics/claude-code` | Canonical coding-agent reference but extreme issue/PR volume; no fresh scouting unless explicitly requested with an ultra-narrow repro lane. |
| `github/CopilotForXcode` | `NO-GO` for upstream PR after [#80](https://github.com/serejaris/corp-opensource/issues/80) / [watch](coding-assistant-context-code-search-scope-2026-05-27.md): official GitHub Xcode assistant reference, but PR template/auto-close workflow says external contributions are not accepted; only discussion/evidence if maintainers explicitly ask. |

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

Latest repo-universe update: [Cycle 35 CUA / UI automation update](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-35-cua-ui-automation.md), after [Cycle 34 workflow / agent platform update](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-34-workflow-agent-platforms.md), [Cycle 33 fresh discovery](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-33-fresh-discovery.md), [Cycle 32 browser/control-plane/code-context/sandbox update](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-32-repo-universe.md), [Cycle 31 coding-agent control-plane / MCP infra update](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-31-repo-universe.md), [Coding assistant context / code-search scope](coding-assistant-context-code-search-scope-2026-05-27.md), [Orchestration / tool / eval framework scope](orchestration-tool-eval-framework-scope-2026-05-27.md), [Low-code agent workflow builder scope](low-code-agent-workflow-builder-scope-2026-05-27.md), [RAG / memory / retrieval infra scope](rag-memory-retrieval-repo-scope-2026-05-27.md), [Observability / eval harness scope](observability-eval-harness-repo-scope-2026-05-27.md), [MCP registry / inspector / servers / spec scope](mcp-ecosystem-repo-scope-2026-05-27.md), [Codex / opencode / Gemini CLI / Daytona scope](high-churn-terminal-runner-scope-2026-05-27.md), [FastMCP / OpenAI Agents / DeepAgents scope](fastmcp-openai-agents-deepagents-scope-2026-05-27.md), [Cline / CUA / Playwright MCP scope](cline-cua-playwright-mcp-scope-2026-05-27.md), [pydantic-ai / OpenHands / E2B top-tier scope](pydantic-openhands-e2b-top-tier-2026-05-27.md), [browser-agent repo scope](browser-agent-repo-scope-2026-05-27.md), [gptme / CAO / OpenACP verify-first scope](gptme-cao-openacp-verify-first-2026-05-27.md), [Qwen Code / Agent Framework scope watch](qwen-code-agent-framework-scouting-2026-05-27.md), [AI SDK / Agents repo scope check](ai-sdk-agents-repo-scope-2026-05-27.md), [Docker MCP scope check](docker-mcp-scope-2026-05-27.md), [Morph SDK generic scope closure](morph-sdk-generic-scope-closure-2026-05-27.md), [OpenClaw/OpenClaude identity scan](openclaw-openclaude-identity-scouting-2026-05-27.md) and [cycle 29](ai-native-popular-repos-paperclip-scouting-2026-05-27-cycle-29-repo-universe.md). Repo-card queue: [repo cards 2026-05-27](repo-cards-ai-native-2026-05-27.md).
