# Fresh AI-native issue discovery cycle 37, 2026-05-28 03:40 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52), selected candidate [#85](https://github.com/serejaris/corp-opensource/issues/85).

Tracker comments: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560685299), candidate [#85](https://github.com/serejaris/corp-opensource/issues/85#issuecomment-4560688379).

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this cycle used the documented fallback: parent live GitHub search/gates, 6 read-only subagents in two waves, then a 3-role status critique.

`next_status: CANDIDATE`

Upstream action count: `0`.

No upstream comment, PR, ping, rerun, or rebase was made. Runner repro was not run because `corp-opensource-runner` is still unavailable from this environment and no lane reached `PR-READY`.

## Scope

Fresh search over popular AI-native / CUA / MCP / agent-tooling repos updated on 2026-05-27 or later. Parent gates inspected candidate issues, adjacent PR searches, contribution docs, repo activity/popularity, and duplicate-covered lanes.

Primary selected lane: [google-gemini/gemini-cli#27503](https://github.com/google-gemini/gemini-cli/issues/27503), tracked internally as [#85](https://github.com/serejaris/corp-opensource/issues/85).

## Parent live gates

- `google-gemini/gemini-cli#27503` is open, unassigned, no comments at gate time, with labels `priority/p1`, `area/agent`, `kind/bug`, `status/bot-triaged`. Repo: `104664` stars, Apache-2.0, default branch `main`, pushed `2026-05-28T03:15:09Z`.
- Adjacent PR search for Gemini found [#25378](https://github.com/google-gemini/gemini-cli/pull/25378), but it is an older Windows ripgrep `EFTYPE` validation PR, currently `DIRTY/CONFLICTING`, and not an exact cover for `git grep --json` fallback.
- `anomalyco/opencode#29650` is open and assigned to `jlongster`, with no exact PR cover found among adjacent session/processor PRs. It is a strong reserve lane but downgated by assignee ownership and the saturated opencode PR queue.
- `mastra-ai/mastra#17189` is exact-covered by open PR [#17140](https://github.com/mastra-ai/mastra/pull/17140), which changes `pricing-registry.ts` and adds dotted OpenRouter model fixture/test coverage.
- `anomalyco/opencode#29646` is exact-covered by open PR [#29645](https://github.com/anomalyco/opencode/pull/29645), which changes OpenAI websocket retry handling and adds `openai-ws.test.ts`.
- `cline/cline#11105`, `OpenHands/agent-canvas#845/#847`, and `microsoft/opentelemetry-distro-python#172` remain lower-priority leads because their repro/process or repo-fit gates are heavier.

## Regression card

For `google-gemini/gemini-cli#27503`, the upstream report gives a concrete failure:

- primary path: `SearchText` starts with an `rg --json` command;
- fallback path: when `rg` fails under Linux Snap confinement, the fallback invokes `git grep` while preserving `--json`;
- observed failure: `git grep --json ...` exits with code `64` because Git does not accept `--json`.

Minimum useful runner task:

- reproduce on current `main`, either by a focused unit/argument-builder test that simulates `rg` failure and asserts the `git grep` fallback drops ripgrep-only flags, or by the full Linux Snap `rg` scenario from the upstream report;
- capture stdout/stderr/exit code for the failing command;
- repeat PR search and contribution/CLA gate before any upstream comment or PR.

## Lane decisions

| Lane | Live result | Decision |
|---|---|---|
| `google-gemini/gemini-cli#27503` | Open P1 agent bug; no exact PR cover; strong issue body and small code-search fallback contract. | Primary internal `CANDIDATE`; next runner target after `probe#568`/existing queue, depending runner fit. |
| `anomalyco/opencode#29650` | Open, assigned `jlongster`; no exact PR found, but dense session/processor neighborhood and active opencode PR saturation. | Reserve `LEAD / WATCH`; no upstream action unless maintainer asks or runner repro proves an uncovered gap after assignee/PR search. |
| `mastra-ai/mastra#17189` | Exact-covered by open PR `#17140`, `MERGEABLE/BLOCKED`, tests/fixtures present. | `WATCH / NO-GO for implementation` while PR is live. |
| `anomalyco/opencode#29646` | Exact-covered by open PR `#29645`, `MERGEABLE/BLOCKED`, websocket retry tests present. | `WATCH / NO-GO for implementation` while PR is live. |
| `cline/cline#11105` | Open VS Code context issue linked to Linear `CLINE-2326`; no PR found, but repro likely needs VS Code extension host and possibly Windows/VS Code 1.122. | `LEAD`; wait triage or runner-friendly repro path. |
| `OpenHands/agent-canvas#845/#847` | Open low-star repo issues around browser panel placeholder and duplicate terminal output; no PR found. | `LEAD / low-fit watch`; verify repo ownership/activity before promotion. |
| `microsoft/opentelemetry-distro-python#172` | Open GenAI telemetry bug; no PR found, but repo/package popularity is low despite Microsoft ownership. | `LEAD`; useful observability/eval reference, not primary Paperclip-like target. |

## 6-role synthesis

- Factology/duplicates: Gemini `#27503` and opencode `#29650` are the only uncovered high-fit lanes. Mastra `#17189` and opencode websocket `#29646` are duplicate-covered by active PRs.
- Process: `CANDIDATE` is internal-only for Gemini. It is not `PR-READY` because current-main fail-before evidence, runner validation, and contribution/CLA gate are missing.
- Actionability: create internal tracker/watch/README updates only. Upstream actions remain blocked.

## 3-role critique

- Factology: `#25378` is adjacent to Gemini search/ripgrep code but different root cause and currently conflicting; it does not cover `git grep --json` fallback.
- Process: internal `CANDIDATE` is allowed because parent live gates are clean and the upstream issue supplies a regression card. Runner absence blocks only `PR-READY` and upstream action.
- Actionability: selected candidate [#85](https://github.com/serejaris/corp-opensource/issues/85); no upstream comment/PR before runner repro, repeat duplicate search, contribution/CLA check, and a new pre-action 3-role critique.
