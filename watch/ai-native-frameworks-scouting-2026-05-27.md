# AI-native frameworks scouting - 2026-05-27

## User Fit

Новый приоритет: не просто “топовые AI repo”, а инфраструктура для AI-native компаний вроде Paperclip:

- agent runtimes;
- task/goals/harnesses;
- browser/computer-use sandboxes;
- eval/replay/test harness;
- memory/skill/workflow frameworks;
- Cline/OpenHands/OpenClaw/openclaude/OpenCode/Hermes-like контур.

`google-gemini/gemini-cli` не считать актуальным default lane без нового сильного сигнала.

## Live GitHub Snapshot

Checked with `gh` on 2026-05-27.

Confirmed repo facts:

| Repo | Stars | Updated/pushed | Why it matters |
|---|---:|---|---|
| `OpenHands/OpenHands` | 74995 | pushed 2026-05-27 | AI-driven development platform; close to AI-native company workflow surface. |
| `cline/cline` | 62376 | pushed 2026-05-27 | Autonomous coding agent as SDK/IDE/CLI; strong Paperclip-adjacent target. |
| `pydantic/pydantic-ai` | 17327 | pushed 2026-05-27 | Agent framework with typed Python ergonomics; likely fixture/test-friendly bugs. |

`topic:ai-agent` search surfaced additional candidates that need validation because star counts and descriptions can be noisy:

- `trycua/cua` — computer-use agent infrastructure and benchmarks.
- `CopilotKit/CopilotKit` — frontend stack/protocol for agents.
- `browser-use/web-ui` — browser agent UI surface.
- `Gitlawb/openclaude` / OpenClaw-like ecosystem — needs identity/repo-fit verification before trust.
- `iOfficeAI/AionUi` — mentions OpenClaw, Hermes Agent, Claude Code, Codex, OpenCode, Gemini CLI; needs validation before use.

OpenClaw/OpenClaude search notes:

- `openclaw/openclaw` appears as the canonical OpenClaw repo in search and must be validated first; search reported an unusually large star count, so do not treat it as trusted impact until repo history/issues/releases are checked.
- `Gitlawb/openclaude` appears in OpenClaude search; needs identity and maintainer-loop verification before using as target.
- Many `openclaw-*` and `openclaude-*` results are tutorials, installers, awesome lists, or thin wrappers. These are not PR targets unless they expose a concrete harness/runtime bug.

Paperclip-adjacent search notes:

- Search mostly found small Paperclip ecosystem repos (`paperclip-mcp`, deployment guides, model watchdogs, templates). Good for ecosystem awareness, but not primary upstream targets unless they expose MCP/worker/harness integration bugs.

Agent harness search notes:

- Search surfaced many new/high-star harness repos (`langchain-ai/deepagents`, `langchain-ai/deepagentsjs`, `trycua/cua`, `Infisical/agent-vault`, `HKUDS/OpenHarness`, etc.).
- Treat this lane as high-noise. Weekly scan must verify real commit history, issue quality, tests, and maintainer response before creating candidate bugs.

## Scouting Rules For This Lane

Before opening candidate bugs:

1. Verify the repo identity and maintainer activity; do not trust stars alone.
2. Prefer harness/test bugs: replay failure, eval fixture, sandbox isolation, browser/computer-use mismatch, tool permission bug, model/provider adapter bug.
3. Require duplicate-race search before any PR; these repos move quickly.
4. Record whether a bug improves our own Paperclip/Hermes/Codex operational loop.

## Next Candidates

| Rank | Repo | First scouting target |
|---:|---|---|
| 1 | `cline/cline` | recent bugs around tool execution, checkpoints, provider adapters, MCP, terminal/browser harnesses |
| 2 | `OpenHands/OpenHands` | sandbox/runtime/eval bugs with small tests |
| 3 | `pydantic/pydantic-ai` | model/provider/tool-call regressions with typed tests |
| 4 | `trycua/cua` | computer-use sandbox and benchmark reproducibility issues |
| 5 | OpenClaw/openclaude-like repos | verify canonical repo before issue scouting |

## Runner / Parallelism Note

Hermes/Codex acceleration should be treated as worker capacity, not as student infrastructure.

- CT `216` remains Hermes-specific smoke/test only.
- General workload target remains `corp-opensource-runner`.
- Additional Codex/Hermes worker container needs `corp-server` issue/runbook, not ad hoc provisioning.
