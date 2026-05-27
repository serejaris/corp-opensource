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
| 1 | `pydantic/pydantic-ai` | [#5679](https://github.com/pydantic/pydantic-ai/issues/5679): preserve `TextContent.metadata` across Vercel AI / AG-UI adapter round-trip after #5678 final CI |
| 2 | `trycua/cua` | [#1725](https://github.com/trycua/cua/issues/1725): Windows Rust recorder missing `click.png` for `element_index` clicks; needs Windows/runtime proof |
| 3 | `cline/cline` | [#10737](https://github.com/cline/cline/issues/10737): `task_progress` leaks into MCP tool args; strong signal but heavier VS Code/e2e repo |
| 4 | `pydantic/pydantic-ai` | [#5671](https://github.com/pydantic/pydantic-ai/issues/5671): Google cached content config bug; clean unit-level provider candidate |
| 5 | `browser-use/browser-use` | [#4580](https://github.com/browser-use/browser-use/issues/4580): MCP startup failure on Windows; watch until Windows runner/repro exists |

## Six-agent Scouting Result

Run on 2026-05-27:

- **Repo-fit:** best repo quality remains `pydantic/pydantic-ai`; best next Paperclip-like repo is `trycua/cua`; `OpenHands/software-agent-sdk` should be promoted for separate validation; `cline/cline` is useful but heavier.
- **Bug-signal:** strongest new candidates are `pydantic-ai#5679`, `trycua/cua#1725`, `cline#10737`, `pydantic-ai#5671`, and `browser-use#4580`.
- **Repro-path:** `pydantic-ai#5679` has the cleanest local repro/test path; `trycua/cua#1725` has a clear issue repro but needs Windows evidence for marker placement.
- **Patchability:** `pydantic-ai#5679` is likely a localized adapter fix; `trycua/cua#1725` is localized but coordinate conversion must be handled carefully.
- **Duplicate-race:** no open duplicate PR found for `pydantic-ai#5679` or `trycua/cua#1725`; no-go duplicates include `pydantic-ai#5672`, `trycua/cua#1722`, `trycua/cua#1501`, and `browser-use#4846`.
- **PR-readiness:** do not open another upstream PR until `pydantic-ai#5678` final CI is known. For pydantic, run owner test files and coverage-aware checks; for CUA, record OS/runtime evidence.

Decision: next implementation candidate is `pydantic-ai#5679` after `#5678` finishes. `trycua/cua#1725` is the next CUA/harness candidate once a Windows runner path is available.

## Runner / Parallelism Note

Hermes/Codex acceleration should be treated as worker capacity, not as student infrastructure.

- CT `216` remains Hermes-specific smoke/test only.
- General workload target remains `corp-opensource-runner`.
- Additional Codex/Hermes worker container needs `corp-server` issue/runbook, not ad hoc provisioning.

## Second Scouting Cycle - 2026-05-27 02:13 -03

Six subagents reran repo-fit, bug-signal, repro-path, patchability, duplicate-race, and PR-readiness over the current top candidates.

Outcome:

| Candidate | Current state | Decision |
|---|---|---|
| `pydantic/pydantic-ai#5671` | Strong bug, but competing PR [#5681](https://github.com/pydantic/pydantic-ai/pull/5681) is now open, ready, mergeable, and CI/coverage/check green. | `WATCH / review only`; do not open duplicate PR. |
| `cline/cline#10737` | Strong MCP/tool boundary bug, but same class appears in `#8256`, `#9684`, `#9919`, `#10426` and stale/conflicting PRs `#9951`, `#9542`, `#8589`. | `DUPLICATE TRIAGE`; do not open fresh PR until existing PR path is resolved. |
| `trycua/cua#1725` | Current `main` appears to include the fix through `#1718` / `#1720`; issue still open. | `VERIFY`; Windows smoke only. |
| `browser-use/browser-use#4580` | Concrete Windows MCP startup bug, but existing PR `#4657` and related startup PRs/issues exist. | `WATCH`; needs Windows repro against current main. |

Operational lesson: when subagents find a clean bug, re-run duplicate search immediately before starting implementation. `pydantic-ai#5671` became occupied during the scouting cycle.

## Third Scouting Cycle - 2026-05-27 02:30 -03

Six subagents reran repo-fit, bug-signal, repro-path, patchability, duplicate-race, and PR-readiness across Tier 1 AI-native repos.

Outcome:

| Candidate | Current state | Decision |
|---|---|---|
| `CopilotKit/CopilotKit#4911` | Published package leaked `workspace:*`; no matching PR found; release-script test surface exists. | `IMPLEMENTED`; upstream PR [#5035](https://github.com/CopilotKit/CopilotKit/pull/5035) opened. |
| `langchain-ai/deepagents` | Best repo-fit, but top simple candidates had assignment/duplicate risk (`#3573` -> `#3574`, `#3441` duplicate race). | `WATCH`; choose only unoccupied issues. |
| `OpenHands/OpenHands#14563` | High relevance to skills/global agents, maintainer confirmed mount gap, but related design/skills PRs exist. | `WATCH / design-race`; no PR without tighter maintainer signal. |
| `browser-use/browser-use#4801` | Relevant browser-agent failure mode, low duplicate risk, but issue needs synthetic repro before patch. | `WATCH / needs-repro`. |
| `E2B#1352` | Fresh sandbox stdout bug, low public duplicate risk, but linked internal Linear. | `WATCH`; wait for public maintainer signal or local SDK repro. |

Operational lesson: small packaging/release bugs can be valid first PRs when they have a strong regression test and low duplicate risk, even if they are less directly “agentic” than runtime bugs. The strategy should still prefer agent/harness bugs, but not ignore clean unblockers in the same ecosystem.

## Fourth Scouting Cycle - 2026-05-27 03:00 -03

Six subagents reran repo-fit, bug-signal, repro-path, patchability, duplicate-race, and PR-readiness after the CopilotKit PR was opened.

Follow-up state:

| Existing work | Current state | Decision |
|---|---|---|
| `pydantic/pydantic-ai#5678` | Open, mergeable, CI/coverage green; no human review yet. | Watch; no code action. |
| `pydantic/pydantic-ai#5680` | Open, mergeable, CI/coverage green; no human review yet. | Watch; no code action. |
| `CopilotKit/CopilotKit#5035` | Open, mergeable; `Vercel docs` green; other Vercel contexts require team authorization for fork deploys. | Watch maintainer review/auth. |
| `cline/cline#10737` | No maintainer response after duplicate-triage comment. | Do not open a fresh duplicate PR. |

New scouting outcome:

| Candidate | Current state | Decision |
|---|---|---|
| `langchain-ai/deepagents#3587` | Fresh subagent/tool-call-id bug on OpenAI-compatible Qwen; no comments, no assignee, no matching PR found. Local checkout created at `/Users/ris/Documents/GitHub/deepagents-3587`; current code raises when `runtime.tool_call_id` is missing. | `CANDIDATE / NEEDS-REPRO`; internal [#20](https://github.com/serejaris/corp-opensource/issues/20). Build synthetic fake-model repro before any PR. |
| `OpenHands/OpenHands#14476` | Resolver conversation metadata race; no comments, no assignee, no matching PR found. | `WATCH / CANDIDATE`; internal [#21](https://github.com/serejaris/corp-opensource/issues/21). Only pursue if DB/service unit repro is possible without SaaS/private infra. |
| `langchain-ai/deepagents#3568` | Very small prompt/schema mismatch, but two participants already offered local fixes in issue comments. | `NO FRESH PR`; avoid racing. |
| `langchain-ai/deepagents#3436` | Repro exists, but comments reframed it as StoreBackend contract/docs/API design. | `NO-GO until maintainer steer`. |
| `OpenHands/OpenHands#14563` | Relevant skills mount gap, but design-race with related skills/issues. | Watch only. |
| `browser-use/browser-use#4801` | Relevant browser-agent a11y/tooling failure, but needs synthetic fixture and may be partly app-side accessibility. | Watch/needs-repro. |

Operational lesson: when a failure is caused by missing `tool_call_id`, do not paper over it in the downstream tool. First prove where the ID is lost and whether a `ToolMessage` can still be matched to the original model tool call.

## Fifth Scouting Cycle - 2026-05-27 - browser-use duplicate gate

Six subagents checked `browser-use/browser-use` after it looked like a good next AI-native harness target.

Outcome:

| Candidate | Current state | Decision |
|---|---|---|
| `browser-use/browser-use#4877` | Strict dependency pins are already covered by open mergeable PR [#4882](https://github.com/browser-use/browser-use/pull/4882), CLA green, CI green. | `WATCH / review only`; do not open duplicate PR. |
| `browser-use/browser-use#4801` | Icon-only button accessibility/selector issue is already covered by open mergeable PR [#4880](https://github.com/browser-use/browser-use/pull/4880), CLA green, CI green; older [#4835](https://github.com/browser-use/browser-use/pull/4835) also exists. | `WATCH / review only`; no fresh PR. |
| `browser-use/browser-use#4846` | MCP CDP lifecycle failure is covered by open mergeable PR [#4881](https://github.com/browser-use/browser-use/pull/4881), CLA green, CI green; [#4847](https://github.com/browser-use/browser-use/pull/4847) also overlaps. | `WATCH / validation only`; no fresh PR. |
| `browser-use/browser-use#4742` | Screenshot blob/context poisoning has open overlapping PRs [#4743](https://github.com/browser-use/browser-use/pull/4743) and [#4751](https://github.com/browser-use/browser-use/pull/4751), plus issue comment claiming work. | `NO FRESH PR`; only compare/review if useful. |
| `browser-use/browser-use#4579` | Remote CDP hangs are already covered by open PRs [#4862](https://github.com/browser-use/browser-use/pull/4862) and [#4875](https://github.com/browser-use/browser-use/pull/4875). | `NO FRESH PR`; only review/validation. |

Decision: `browser-use` is useful to watch, but not a clean new PR lane right now. Best next lane is `OpenHands/OpenHands`, where maintainer response is fresher and there are small issue paths without the same duplicate pressure.

Operational lesson: duplicate search must be repeated immediately before implementation. A candidate that looked clean in scouting can become occupied by an open mergeable PR with tests; in that case the right contribution is validation/review/watch, not another PR.
