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

## Tier 1: Core External Targets

These are allowed for regular weekly scouting.

| Repo | Current live signal | Why in scope |
|---|---|---|
| `cline/cline` | `62376` stars, pushed `2026-05-27` | Coding-agent IDE/CLI runtime, MCP/tool/provider bugs, checkpoints, browser automation. |
| `OpenHands/OpenHands` | `74995` stars, pushed `2026-05-27` | AI-driven development platform, skills, resolver, sandbox, browser, self-hosting, worker lifecycle. |
| `pydantic/pydantic-ai` | `17327` stars, pushed `2026-05-27` | Typed agent framework, providers/tools/model settings, strong fixture/test culture. |
| `trycua/cua` | active bug queue on `2026-05-27` | Computer-use agent infrastructure, sandboxes, benchmarks, trajectory recording. |
| `browser-use/browser-use` | `95723` stars, pushed `2026-05-26` | Browser automation for agents; replay/DOM/screenshot/tooling bugs. |
| `CopilotKit/CopilotKit` | `31776` stars, pushed `2026-05-27` | Frontend stack/protocol for agents and generative UI. |
| `langchain-ai/deepagents` | `23391` stars, pushed `2026-05-27` | Agent harness with LangChain maintainership; likely tool/memory/runtime tests. |
| `e2b-dev/E2B` | `12371` stars, pushed `2026-05-27` | Secure sandbox/runtime layer for agents. |

## Tier 2: Watch / Verify Before Bug Scouting

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
