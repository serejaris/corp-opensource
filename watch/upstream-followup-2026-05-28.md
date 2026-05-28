# Upstream follow-up, 2026-05-28

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Affected internal trackers: [#85](https://github.com/serejaris/corp-opensource/issues/85), [#82](https://github.com/serejaris/corp-opensource/issues/82), [#81](https://github.com/serejaris/corp-opensource/issues/81), [#80](https://github.com/serejaris/corp-opensource/issues/80), [#58](https://github.com/serejaris/corp-opensource/issues/58), [#56](https://github.com/serejaris/corp-opensource/issues/56), [#53](https://github.com/serejaris/corp-opensource/issues/53).

Tracker comments: [#82 demotion](https://github.com/serejaris/corp-opensource/issues/82#issuecomment-4559803855), [#52 synthesis](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4559804608), [#53 mini-swe refresh](https://github.com/serejaris/corp-opensource/issues/53#issuecomment-4559839933), [#52 mini-swe synthesis](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4559840785), [#52 settlement refresh](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560463593), [#52 active PR dashboard refresh](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560641484), [#52 cycle 37 Gemini candidate](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560685299), [#52 runner backlog refresh](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560707233), [#85 Gemini refresh](https://github.com/serejaris/corp-opensource/issues/85#issuecomment-4560707690), [#52 04:04 refresh](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560746495), [#52 04:17 dashboard correction](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560803072), [#86 Vercel #15652 refresh](https://github.com/serejaris/corp-opensource/issues/86#issuecomment-4561045571), [#52 05:19 bounded refresh](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561046486).

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this follow-up used the documented fallback: parent live GitHub gates, 6 read-only subagents, then a 3-role critique.

No upstream PR/comment/ping was made.

## Vercel #15652 bounded refresh, 2026-05-28 05:19 UTC

Bounded refresh over active runner candidates and fresh AI-native/MCP leads. Parent live gates used `gh issue view`, targeted `gh pr list` duplicate search, runner/tooling checks, 6 read-only fallback roles, and a 3-role critique. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are still unavailable here, so this used the documented fallback.

`next_status: CANDIDATE`

Upstream action count: `0`.

Runner action count: `0`.

Tracker comments: Vercel [#86](https://github.com/serejaris/corp-opensource/issues/86#issuecomment-4561045571), umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561046486).

Runner gate remains closed via [#10](https://github.com/serejaris/corp-opensource/issues/10): no `corp-opensource-runner` is provisioned; local env has Node `v20.19.2` and Python `3.13.5`, but no `pnpm`, `bun`, `cargo`, `rustc`, or Docker.

| Lane | Live result | Decision |
|---|---|---|
| `vercel/ai#15652` | Open, unassigned. New live upstream signal: [issuecomment-4561014660](https://github.com/vercel/ai/issues/15652#issuecomment-4561014660) narrows the likely safe behavior/test framing: preserve the current `jsonTool` default, honor explicit `disableParallelToolUse: false`, and assert the serialized Anthropic request carries `disable_parallel_tool_use: false`; omitted option should still serialize `true`. Exact open PR cover still not found by parent/subagent duplicate gates. Adjacent `#15112` and `#10274` remain provider/tool churn risks, not exact covers. | Keep `CANDIDATE / needs-runner-fixture`; not `PR-READY` until current-main package fixture, contribution/checks/changeset gate, repeat duplicate search, and pre-action critique. |
| `google-gemini/gemini-cli#27503` | Open, unassigned, labels `priority/p1`, `area/agent`, `kind/bug`, no comments. Exact open PR cover not found for `#27503`, `SearchText`, or `git grep --json`; old `#19638` is SearchText result-cap/context-overflow work and not an exact cover. | Keep high strategic `CANDIDATE`; runner/current-main arg-builder repro required. |
| `iOfficeAI/AionUi#3074` | Open, unassigned, no comments. Exact STT multipart cover not found in `AionUi` or `AionCore`. | Keep `CANDIDATE-needs-runner-repro`. |
| `probelabs/probe#568` | Open, unassigned, labels `bug`/`external`; Visor/probelabs comment confirms plausible JSON stdout contamination and asks for exact CLI/API/env. Exact open PR cover not found. | Keep first operational runner target; no upstream reply without runner evidence. |
| `iOfficeAI/AionUi#3051` | Open, unassigned, no comments. Exact Unicode/ACP cover not found; `AionCore#353` is same-file stream relay adjacency but not exact cover. | Keep `CANDIDATE-needs-runner-repro / adjacent file race`. |
| `langchain-ai/langgraph#7915` | Open, unassigned, label `external`; reporter says they have a tested fix and asks assignment. Exact open PR cover not found. | Keep `CANDIDATE-with-ownership-risk / WATCH`; no comment/PR unless maintainer signal or stale-window changes. |
| `pydantic/pydantic-ai#5696` | Open, unassigned, labels `bug`, `durable exec`, `pydanty:bug`, `serialization`; pydanty timed out and asks maintainer attention. Exact open PR cover not found. | Keep `WATCH / pydanty-runtime-timeout`; do not enter automation-owned lane. |

Fresh leads checked by the factology role:

- `microsoft/playwright-mcp#1636`: open/unassigned/no comments and no exact memory/OOM PR found, but this is runner-heavy browser profiling and remains a long-run `WATCH`, not a small fix lane.
- `modelcontextprotocol/go-sdk#977`: open and no open PR cover found, but issue author self-claimed willingness to take the change; keep race-prone `WATCH`.
- `QwenLM/qwen-code#3718`: open and maintainer-confirmed for one `qwen mcp remove` path, but contributor self-claim creates duplicate race; keep `WATCH`.
- `microsoft/agent-framework#5876` and `Kilo-Org/kilocode#10149`: duplicate-covered by open PRs `#5931` and `#10169`.

6-role synthesis:

- Duplicate/race: no exact cover was found for current core candidates, but ownership/automation gates block LangGraph and pydantic-ai; Vercel has adjacent provider/tool churn, not exact cover.
- Process: internal tracker refresh is allowed because Vercel gained a material framing comment; upstream PR/comment remains blocked.
- Scope: Gemini remains the strongest terminal-agent lane; BrowserOS/Agent-S remain stronger Paperclip-native browser/MCP/CUA reserves; Probe stays the cheapest first runner proof.
- Tracker/delta: update only Vercel [#86](https://github.com/serejaris/corp-opensource/issues/86) and umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52), plus this watch note/README; do not create new tracker issues.

3-role critique:

- Factology/duplicates: approved as `CANDIDATE / no exact cover`; do not overclaim Vercel semantics beyond request serialization.
- Process gates: no upstream action, no runner action, no PR-ready promotion.
- Actionability: runner order remains operationally conservative: Probe first, Gemini second, then Serena/Agent-S/Midscene/BrowserOS; Vercel/Aion/LangGraph follow when runner capacity or maintainer signal makes them sharper.

## Active PR dashboard correction, 2026-05-28 04:17 UTC

Bounded refresh over the active upstream PR dashboard only: `OpenHands/software-agent-sdk#3394`, `anomalyco/opencode#29565/#29530`, `cline/cline#11087`, `CopilotKit/CopilotKit#5035`, `pydantic/pydantic-ai#5680/#5678`, and stale `NousResearch/hermes-agent#15640`.

Process: parent live gates, 6 read-only fallback roles, then 3-role critique. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are still unavailable here, so this used the documented fallback. Runner repro was not required because this pass only updates read-only dashboard state and no lane reached `PR-READY`.

`next_status: WATCH`

Upstream action count: `0`.

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560803072).

No upstream comment, PR, ping, rerun, rebase, close, local/heavy repro, or per-PR tracker comment was made.

| Lane | Live result | Next action |
|---|---|---|
| `OpenHands/software-agent-sdk#3394` | Open, non-draft, `MERGEABLE/BLOCKED`, old `CHANGES_REQUESTED`; status rollup empty; no new maintainer eval/re-review visible after follow-up `3c8eae78`. | Wait maintainer eval / re-review / workflow approval. No repeat comment. |
| `opencode#29565` | Open, non-draft, `MERGEABLE/BLOCKED`; visible duplicate/standards/compliance/contributor gates green; no review. Adjacent `#29632` may overlap retry/format semantics, but exact cover is not proven. | Wait maintainer review; watch `#29632`; no third opencode PR without distinct signal. |
| `opencode#29530` | Open, non-draft, `MERGEABLE/BLOCKED`; visible duplicate/standards/compliance gates green; no review. | Wait maintainer review. |
| `cline#11087` | Open, non-draft, `MERGEABLE/BLOCKED`; visible SDK quality, Ubuntu/Windows tests and Socket checks green/skipped; no maintainer review. | Wait maintainer review. |
| `CopilotKit#5035` | Open, non-draft, `MERGEABLE/BLOCKED`, `REVIEW_REQUIRED`; docs Vercel preview green; other Vercel contexts are team-authorization gated, not proven code failures. | Wait maintainer review / team preview authorization; no guidance ping without maintainer ask. |
| `pydantic-ai#5680` | Open, non-draft, `MERGEABLE/CLEAN`; broad visible CI/harness/coverage green; no maintainer review. Adjacent `#5602/#5605` are same-surface overlap risks, not exact covers. | Wait maintainer/bot review; recheck if `#5602/#5605` merge first. |
| `pydantic-ai#5678` | Open, non-draft, `MERGEABLE/CLEAN`; broad visible CI/harness/coverage green; no maintainer review. | Wait maintainer/bot review. |
| `Hermes#15640` | Open, non-draft, current live gate `MERGEABLE/CLEAN`; no comments/reviews/checks; stale since `2026-04-25`. Adjacent `#29673` is open/non-draft `MERGEABLE/UNSTABLE` and overlaps gateway/launchd profile naming, but is not a proven replacement. | Keep `PR-OPEN / WATCH-stale-clean`; no ping/close/rebase without fresh macOS validation or maintainer signal; watch `#29673`. |

6-role synthesis:

- Factology: `MERGEABLE` is not merge-ready when paired with `BLOCKED`, `CHANGES_REQUESTED`, or `REVIEW_REQUIRED`; timestamp all live claims.
- Duplicate/race: `opencode#29632`, `pydantic-ai#5602/#5605`, and `Hermes#29673` are overlap/watch-only, not exact covers. CopilotKit Vercel auth contexts are infra/team authorization risk, not duplicate coverage.
- Process: one umbrella `#52` heartbeat is enough. Per-PR internal comments and upstream comments would be noise because there is no material status transition.
- Actionability: one role proposed CopilotKit/pydantic nudging, but the critique rejected it: no maintainer ask, no actionable failing CI, no conflict, no exact supersede, no approval/merge transition, and no fresh runner validation.

3-role critique:

- Factology/duplicates: approved after correcting Hermes wording from the 04:04 `UNKNOWN/UNKNOWN` snapshot to current `#15640 MERGEABLE/CLEAN` and `#29673 MERGEABLE/UNSTABLE`; `#29673` remains collision pressure, not exact replacement.
- Process gates: internal README/watch update plus one `#52` correction comment is allowed; upstream ping/rerun/rebase/close/PR is not.
- Actionability: final dashboard status stays `WATCH / monitor-only`; no lane is `COMMENT-FIRST`, `PR-READY`, or `NO-GO` in this refresh.

## Runner candidates + active PR dashboard refresh, 2026-05-28 04:04 UTC

Bounded refresh over the same six runner candidates plus the active PR dashboard. Parent live gates used `gh issue view`, `gh pr view`, and targeted PR search. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are still unavailable here, so this used the documented fallback: 6 read-only roles, then 3-role critique.

`next_status: CANDIDATE`

Upstream action count: `0`.

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560746495).

No upstream comment, PR, ping, rerun, rebase, close, or local/heavy repro was made. Runner gate remains closed via [#10](https://github.com/serejaris/corp-opensource/issues/10); CT216 remains Hermes-only.

| Area | Live result | Decision |
|---|---|---|
| Runner candidates | `probelabs/probe#568`, `google-gemini/gemini-cli#27503`, `oraios/serena#1519`, `simular-ai/Agent-S#195`, `web-infra-dev/midscene#2544`, and `browseros-ai/BrowserOS#1005` all remain open. Targeted PR search found no exact or super-strong cover. | Keep backlog `CANDIDATE`; none is `PR-READY` without current-main runner evidence, contribution/process gate, and repeat duplicate search. |
| Active upstream PRs | No merge, close, approval, maintainer ask, actionable failing CI, or exact supersede transition was found. Current recheck: `opencode#29565/#29530` are open/non-draft `MERGEABLE/BLOCKED`; `Hermes#15640` and overlap `#29673` currently return `UNKNOWN/UNKNOWN`. | Keep dashboard `WATCH / monitor-only`; do not treat mergeability API uncertainty as a rerun/rebase/close signal. |
| Runner priority | Probe remains the first operational runner target because it has the lowest setup churn and preserves tracker continuity. Gemini is the highest strategic/race-sensitive candidate because the issue is P1/agent/bug in a very popular CLI, but it stays behind a current-main repro and duplicate-race gate. | Operational order unchanged: Probe first, Gemini second; record the strategic caveat without churn. |

6-role synthesis:

- Factology: no runner target closed, gained assignee/comment transition, or acquired an exact PR cover.
- Duplicate/race: Gemini has the highest race pressure because of P1 label and active repo; BrowserOS has the heaviest adjacent MCP queue; Probe/Agent-S/Midscene currently have lower exact-cover pressure.
- Process: one umbrella tracker comment is enough; per-lane tracker comments would be noise because status/order did not materially change.
- Actionability: active PR dashboard stays monitor-only. No upstream action is justified without maintainer ask, actionable failing CI, conflict, exact supersede, approval/merge transition, or fresh runner validation.

3-role critique:

- Factology/duplicates: approved after correcting wording to avoid overclaiming transient GitHub mergeability values. Current `opencode#29565/#29530` view is `MERGEABLE/BLOCKED`; Hermes remains conservative because both stale PR `#15640` and overlap `#29673` returned `UNKNOWN/UNKNOWN` in the final parent recheck.
- Process: internal README/watch/repo-scope update plus one `#52` heartbeat is allowed; upstream comment/PR/rerun/rebase is not.
- Actionability: `Probe first` means operational runner order only. `Gemini highest strategic/race-sensitive` is a caveat, not PR readiness.

## Runner-candidate backlog refresh, 2026-05-28 03:50 UTC

Bounded refresh over current runner candidates and practical runner targets: `probelabs/probe#568`, `google-gemini/gemini-cli#27503`, `oraios/serena#1519`, `simular-ai/Agent-S#195`, `web-infra-dev/midscene#2544`, and `browseros-ai/BrowserOS#1005`.

Process: parent live issue views, duplicate PR searches, runner/tooling gate, 6 read-only roles in two waves, then 3-role critique. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are still unavailable here, so this used the documented fallback.

`next_status: CANDIDATE`

Upstream action count: `0`.

Tracker comments: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560707233), Gemini [#85](https://github.com/serejaris/corp-opensource/issues/85#issuecomment-4560707690).

Runner gate remains closed: [#10](https://github.com/serejaris/corp-opensource/issues/10) is still open and says no `corp-opensource-runner` has been provisioned. Local tooling is insufficient for bounded repro work: Node `v20.19.2` and Python `3.13.5` are present, but `pnpm`, `bun`, `cargo`, and `rustc` are unavailable.

No upstream comment, PR, ping, rerun, or rebase was made.

| Runner order | Lane | Live result | Next gate |
|---:|---|---|---|
| 1 | `probelabs/probe#568` | Open, labels `bug`/`external`, no exact PR cover found. Correction: this lane is JSON stdout pollution by debug/progress lines such as `Pattern:`, `Path:`, and BM25 text, not a `probe.toml`/config issue. | Run focused CLI + Node SDK current-main repro; capture stdout/stderr/exit/JSON parse result. |
| 2 | `google-gemini/gemini-cli#27503` | Open P1 `area/agent` bug; adjacent PR `#25378` remains Windows ripgrep EFTYPE-only, `DIRTY/CONFLICTING`, and not an exact cover for `git grep --json` fallback. | Run current-main repro or focused fallback arg-builder test; verify contribution/CLA gate. |
| 3 | `oraios/serena#1519` | Open, unassigned, no exact PR found for Docker host-port dashboard 403. | Docker/current-main repro with security-preserving Host validation test. |
| 4 | `simular-ai/Agent-S#195` | Open, unassigned, no exact PR found for `WindowsACI.hotkey("enter")` splitting into characters. | Unit/current-main repro for single-key vs chord generation; GUI smoke only if needed. |
| 5 | `web-infra-dev/midscene#2544` | Open, unassigned, no exact PR found for web `longPress` 600ms cap. | Browser/Playwright timing repro before promotion from `WATCH / runner target`. |
| 6 | `browseros-ai/BrowserOS#1005` | Open; reporter's `2026-05-26` update gives sharper root cause around byte-identical full tool re-registration on every tool call/focus/connect. Adjacent MCP PRs `#1031/#948/#944/#864/#655/#786/#1038` are not exact `list_changed` dedupe covers. | MCP/browser runtime repro and tool-list hash/dedupe design gate. |

6-role synthesis:

- No target issue was closed or exact-covered in this refresh.
- The practical runner order starts with `probelabs/probe#568` because it is the cheapest CLI/stdout contract to prove without browser, GUI, external API, Docker, Bun, or Rust toolchain assumptions.
- `google-gemini/gemini-cli#27503` remains the strongest high-signal terminal-agent candidate, but its full repro is heavier because it involves `SearchText` fallback and possibly Linux Snap `rg`; a focused arg-builder test may be enough for the first pass.
- `Serena#1519`, `Agent-S#195`, `Midscene#2544`, and `BrowserOS#1005` remain valid but progressively heavier runner lanes.

3-role critique:

- Factology: the only correction is the Probe wording; use `probelabs/probe#568` and describe JSON stdout contamination, not config/probe.toml.
- Process: internal backlog refresh with `next_status: CANDIDATE` is allowed; runner absence blocks `PR-READY` and all upstream actions, not internal tracker/watch updates.
- Actionability: comment only umbrella `#52` and fresh primary tracker `#85`; do not comment upstream or unchanged reserve trackers `#80/#84/#81/#72`.

## Fresh AI-native issue discovery cycle 37, 2026-05-28 03:40 UTC

Detailed note: [cycle 37 Gemini / opencode](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-37-gemini-opencode.md).

Process: parent GitHub search over popular AI-native / CUA / MCP / agent-tooling repos, live issue/PR/contribution gates, 6 read-only roles in two waves, then 3-role status critique. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are still unavailable here, so this used the documented fallback.

`next_status: CANDIDATE`

Upstream action count: `0`.

Tracker comments: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560685299), selected candidate [#85](https://github.com/serejaris/corp-opensource/issues/85).

No upstream comment, PR, ping, rerun, or rebase was made.

| Lane | Live result | Decision |
|---|---|---|
| `google-gemini/gemini-cli#27503` | Open, unassigned, no comments at gate time; labels `priority/p1`, `area/agent`, `kind/bug`, `status/bot-triaged`. Adjacent PR `#25378` is Windows ripgrep EFTYPE validation, `DIRTY/CONFLICTING`, and not an exact cover for `git grep --json` fallback. | Primary internal `CANDIDATE` via [#85](https://github.com/serejaris/corp-opensource/issues/85). Needs current-main runner repro or focused fallback arg-builder test, contribution/CLA gate and repeat duplicate search before upstream action. |
| `anomalyco/opencode#29650` | Open and assigned to `jlongster`; no exact PR found among adjacent session/processor PRs. | Reserve `LEAD / WATCH`; no upstream action unless maintainer asks or runner repro proves an uncovered gap after assignee/PR search. |
| `mastra-ai/mastra#17189` | Exact-covered by open PR `#17140`, which changes `pricing-registry.ts` and adds dotted OpenRouter model fixture/test coverage. | `WATCH / NO-GO for implementation` while `#17140` is live. |
| `anomalyco/opencode#29646` | Exact-covered by open PR `#29645`, which changes OpenAI websocket retry handling and adds `openai-ws.test.ts`. | `WATCH / NO-GO for implementation` while `#29645` is live. |
| `cline/cline#11105` | Open VS Code context issue linked to Linear `CLINE-2326`; no exact PR found, but repro likely needs VS Code extension host and possibly Windows/VS Code 1.122. | `LEAD`; wait triage or runner-friendly repro path. |
| `OpenHands/agent-canvas#845/#847` | Open low-star repo issues around browser panel placeholder and duplicate terminal output; no PR found. | `LEAD / low-fit watch`; verify repo ownership/activity before promotion. |
| `microsoft/opentelemetry-distro-python#172` | Open GenAI telemetry bug; no PR found, but repo/package popularity is low despite Microsoft ownership. | `LEAD`; useful observability/eval reference, not primary Paperclip-like target. |

6-role synthesis: Gemini `#27503` is the best fresh internal candidate because it is popular, AI-native, labeled P1/agent/bug, and uncovered. Opencode `#29650` is a strong practical reserve but gated by maintainer assignment and saturated same-day opencode queue. Mastra `#17189` and opencode websocket `#29646` are duplicate-covered by active PRs.

3-role critique: internal `CANDIDATE` is allowed because parent live gates are clean and the upstream issue contains a regression card. It is not `PR-READY`: runner/current-main fail-before evidence, contribution/CLA check, repeat duplicate search, and pre-action critique are still required.

## Active PR dashboard refresh, 2026-05-28 03:35 UTC

Bounded active-PR dashboard refresh over tracked upstream PRs: `OpenHands/software-agent-sdk#3394`, `anomalyco/opencode#29565/#29530`, `cline/cline#11087`, `CopilotKit/CopilotKit#5035`, `pydantic/pydantic-ai#5678/#5680`, and stale `NousResearch/hermes-agent#15640`.

Process: parent live `gh pr view` gates, adjacent PR searches, 6 read-only roles in two waves, then 3-role critique. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are still unavailable here, so this was the documented fallback. Runner repro was not required because this pass only updates read-only dashboard state and no lane reached `PR-READY`.

`next_status: WATCH`

Upstream action count: `0`.

No upstream comment, PR, ping, rerun, rebase, close, or force-push was made.

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560641484).

| Lane | Live result | Next action |
|---|---|---|
| `OpenHands/software-agent-sdk#3394` | Open, non-draft, `MERGEABLE/BLOCKED`, old `CHANGES_REQUESTED`; status rollup empty; maintainer eval/re-review still not visible after follow-up `3c8eae78`. | Wait maintainer eval / re-review / workflow approval. No repeat comment. |
| `opencode#29565` | Open, non-draft, `MERGEABLE/BLOCKED`; duplicate/standards/compliance/contributor checks green; no review. Adjacent `#29632` changes structured-output retry/format flow and may interact semantically, but is not an exact cover. | Wait maintainer review; watch `#29632`; no third opencode PR without distinct signal. |
| `opencode#29530` | Open, non-draft, `MERGEABLE/BLOCKED`; duplicate/standards/compliance/contributor-management checks green, but not full test CI; no review. | Wait maintainer review. |
| `cline#11087` | Open, non-draft, `MERGEABLE/BLOCKED`; visible SDK quality, Ubuntu/Windows Node 24 tests, and Socket checks green/skipped; Greptile gap already answered. | Wait maintainer review; no new provider-routing work. |
| `CopilotKit#5035` | Open, non-draft, `MERGEABLE/BLOCKED`, `REVIEW_REQUIRED`; docs Vercel preview green; other Vercel contexts fail through team authorization URLs, not proven code failures. Release/deps PRs are timing watch, not supersede. | Wait maintainer review / Vercel team authorization. |
| `pydantic-ai#5678` | Open, non-draft, `MERGEABLE/CLEAN`; broad CI/harness/coverage/smokeshow green; no approval. | Wait maintainer/bot review. |
| `pydantic-ai#5680` | Open, non-draft, `MERGEABLE/CLEAN`; broad CI/harness/coverage/smokeshow green; no approval. Same-file overlap risk with open `#5602/#5605`, but neither is an exact supersede. | Wait maintainer/bot review; recheck if `#5602/#5605` merge first. |
| `Hermes#15640` | Open, non-draft, `MERGEABLE/CLEAN`; no comments/reviews/checks; stale since `2026-04-25`. Fresh `#29673` is gateway/launchd naming overlap risk and possible future supersede pressure, but not a proven replacement yet. | Keep `PR-OPEN / WATCH-stale-clean`; no ping/close/rebase without fresh macOS validation or maintainer signal; watch `#29673`. |

6-role synthesis:

- No tracked PR was merged, closed, approved, superseded, or given a new maintainer ask.
- Live wording updates from the 03:10 dashboard: `opencode#29565` and `opencode#29530` moved from `UNKNOWN/UNKNOWN` to `MERGEABLE/BLOCKED`; `Hermes#15640` moved from `UNKNOWN/UNKNOWN` to `MERGEABLE/CLEAN`.
- Exact supersede was not found. Watch-only overlaps remain: `opencode#29632`, `pydantic-ai#5602/#5605`, CopilotKit release/deps PRs, and `Hermes#29673`.
- There is no actionable failing CI. CopilotKit Vercel failures are team-authorization gated, skipped Cline checks are not failures, and empty Hermes/OpenHands rollups are not rerun triggers from this repo.

3-role critique:

- Factology/duplicates: no exact cover or closure transition was found; `Hermes#29673` is high collision risk but still a broader rewrite path, not a replacement for `#15640` today.
- Process: internal dashboard update and one umbrella heartbeat are enough; per-PR tracker comments and upstream comments would be noise.
- Actionability: no comment, rerun, rebase, ping, close, or new PR is warranted until maintainer ask, actionable failing CI, conflict, exact supersede, approval/merge transition, or fresh runner validation.

## Duplicate-covered settlement refresh, 2026-05-28 02:57 UTC

Bounded settlement refresh over duplicate-covered lanes: `modelcontextprotocol/typescript-sdk#2115`, `stablyai/orca#2930`, `browser-use/browser-harness#382`, `colbymchenry/codegraph#507`, `pydantic/pydantic-ai#5688`, and `e2b-dev/E2B#1349`.

Process: parent live GitHub gates, 6 read-only roles in two waves, then 3-role critique-style factology/process/actionability review. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are still unavailable here, so this used the documented fallback.

`next_status: WATCH / settlement refresh`

Upstream action count: `0`.

No upstream comment, PR, ping, rerun, rebase, close, or runner repro was made. Runner was not required because this pass only records live duplicate/settlement state and no lane reached `PR-READY`.

| Lane | Live result | Decision |
|---|---|---|
| `modelcontextprotocol/typescript-sdk#2115` | Issue open. Exact duplicate PRs [#2116](https://github.com/modelcontextprotocol/typescript-sdk/pull/2116) and [#2138](https://github.com/modelcontextprotocol/typescript-sdk/pull/2138) are open/non-draft with green checks, but live state is `BLOCKED` / review-required. | `WATCH / duplicate-covered`; do not overclaim merge-ready. Re-enter only if both PRs close unmerged, maintainer asks, or merged fix misses `requestId: 0`. |
| `stablyai/orca#2930` | Issue open. Exact-cover PR [#2948](https://github.com/stablyai/orca/pull/2948) is open/non-draft and changes the SSH stream reader plus short-final-chunk regression; live state is `UNSTABLE` with only the visible community-tracking check green. | `WATCH / duplicate-covered`; watch merge/close/stall/gap. |
| `browser-use/browser-harness#382` | Issue open with `in-progress`. Primary exact-cover PR [#346](https://github.com/browser-use/browser-harness/pull/346) is open/non-draft and clean with visible checks green; [#332](https://github.com/browser-use/browser-harness/pull/332) / [#297](https://github.com/browser-use/browser-harness/pull/297) are adjacent keyboard-helper overlap. | `WATCH / duplicate-covered`; no new PR while #346 is live. |
| `colbymchenry/codegraph#507` | Issue open. Exact behavioral cover [#345](https://github.com/colbymchenry/codegraph/pull/345) is open/non-draft, but live state is `DIRTY` / review-required with no visible checks. | `WATCH / stale-conflicting duplicate-covered`; not settled, but no competing PR while #345 is live unless it closes or maintainer asks. |
| `pydantic/pydantic-ai#5688` | Issue open. Exact-cover PR [#5694](https://github.com/pydantic/pydantic-ai/pull/5694) is open/non-draft, clean, and has broad green CI with only bot/release jobs skipped. | `WATCH / duplicate-covered`; wait maintainer/bot review. |
| `e2b-dev/E2B#1349` | Issue closed `2026-05-27T21:39:52Z`. Superset PR [#1355](https://github.com/e2b-dev/E2B/pull/1355) merged `2026-05-27T20:57:03Z`, approved, and covers JS + Python SDK multi-source Dockerfile parsing. | `SETTLED / resolved by merged #1355`; no further action on closed #1354/#1349 unless regression reopens. |

6-role synthesis:

- All non-E2B lanes remain covered by live upstream PRs; none justify a competing comment or PR.
- E2B is the only settlement transition: `#1355` merged and `#1349` is closed fixed, so update README/watch from `WATCH #1355` to `SETTLED`.
- Codegraph remains watch, not settled, because the exact-cover PR is currently dirty/conflicting.
- Browser-harness remains watch despite maintainer PR-welcome signal because `#346` is now the primary exact cover.
- MCP TS and Orca wording must stay conservative: MCP is review-gated, and Orca has light visible checks plus `UNSTABLE`.

3-role critique:

- Factology/duplicates: no lane is uncovered enough for upstream action; avoid overclaims on MCP mergeability, Orca readiness, or codegraph cleanliness.
- Process: internal README/watch/tracker update is allowed; upstream comments are unnecessary and would add noise to already covered lanes.
- Actionability: runner repro only becomes relevant if a cover PR closes unmerged, lands incomplete, stalls with maintainer signal, or the issue reopens.

## Scope

Bounded follow-up over six active candidate/watch lanes:

| Lane | Internal tracker | Live follow-up result | Status |
|---|---|---|---|
| `stablyai/orca#2930` | `#82` | Exact open non-draft upstream PR [#2948](https://github.com/stablyai/orca/pull/2948) now covers short SSH stream chunks / zero-filled reads. | `WATCH / duplicate-covered` |
| `browseros-ai/BrowserOS#1005` | `#81` | Open, no exact PR found, but no runner-backed MCP registry/list_changed repro in this cycle. | `CANDIDATE` |
| `probelabs/probe#568` | `#80` | Open, maintainer/probelabs interest exists, no exact PR found; still missing exact CLI/API/env evidence. | `CANDIDATE` |
| `mastra-ai/mastra#17118` | `#58` | Open and triaged with `impact:medium` / `effort:medium`; no exact PR, no terminal-width MRE card yet. | `CANDIDATE` |
| `ag-ui-protocol/ag-ui#1635` | `#56` | Open, no exact PR, but assignment/process and nearby Mastra adapter collision risk block implementation-first action. | `COMMENT-FIRST after runner evidence` |
| `SWE-agent/mini-swe-agent#826` | `#53` | Open, no exact PR found; needs current-main runner repro after adjacent `#832`. | `WATCH` |

## Parent live gates

- `stablyai/orca#2930` is still open, but [#2948](https://github.com/stablyai/orca/pull/2948) is open, non-draft, and titled `fix: reject short SSH stream chunks instead of zero-filling reads (#2930)`.
- `#2948` changes `src/main/ssh/ssh-filesystem-stream-reader.ts` and `src/main/providers/ssh-filesystem-provider-stream.test.ts`; the commit says `Closes #2930` and adds a short-final-chunk rejection regression.
- BrowserOS, Probe, Mastra, AG-UI, and mini-swe lanes did not get exact covering PRs in this pass, but none gained runner-backed evidence sufficient for upstream action.
- Runner was not used because no lane reached `PR-READY`; Orca stopped at duplicate gate.

## 6-subagent synthesis

- Factology/live-state: only hard status change is `orca#2930 -> WATCH / duplicate-covered`; other lanes stay blocked on runner evidence or process gates.
- Duplicate coverage: `orca#2948` exactly covers `#2930`; BrowserOS, Probe, Mastra, AG-UI, and mini-swe have no exact PR in the checked set.
- Actionability: upstream action count should remain `0`; no comment/PR without runner-backed evidence, and no Orca duplicate PR while `#2948` is live.
- Process/tracker consistency: README, repo-card, scope row, and `watch/orca-2930-short-stream-zero-fill.md` were stale because they still said `CANDIDATE`.
- Runner prioritization: `mini-swe-agent#826` is the best next runner target after Orca becomes duplicate-covered; `probelabs/probe#568` is the compact fallback if mini-swe runner setup blocks.
- Final synthesis: choose exactly `WATCH` for this bounded follow-up.

## 3-role critique

- Factology/duplicates: holding `#82` as `CANDIDATE` after `#2948` would create duplicate-race risk; `PR-OPEN` is not our status because the open PR is upstream-owned.
- Process gates: status change to `WATCH / duplicate-covered` is allowed after parent live gates and 6-subagent scouting; runner is unnecessary because the lane stops before `PR-READY`.
- Actionability: no upstream comment or PR is justified for any lane in this cycle. `AG-UI#1635` can only become `COMMENT-FIRST` after runner-backed evidence; `probe#568` needs exact CLI/API/env results before answering upstream clarification.

## Decision

`next_status: WATCH`

Upstream action count: `0`.

`stablyai/orca#2930` / internal `#82` is demoted from `CANDIDATE` to `WATCH / duplicate-covered by upstream #2948`. Other internal statuses remain unchanged.

Internal tracker `#56` was also body-aligned from plain `CANDIDATE` to `COMMENT-FIRST after runner-backed test card`; this was a consistency update only and did not authorize upstream action.

Next runner target: `SWE-agent/mini-swe-agent#826`. Fallback runner target: `probelabs/probe#568`.

Mini-swe continuation, 2026-05-28 UTC: a dedicated 6-role follow-up plus 3-role critique for `SWE-agent/mini-swe-agent#826` kept the same final `next_status: WATCH`. Live gates found no exact covering PR and confirmed current `local.py` still uses `subprocess.run(..., shell=True, timeout=...)`, but `corp-opensource-runner` is still not provisioned per `#10`, `ssh corp-server` does not resolve here, and CT216 is Hermes-only. No upstream comment/PR and no local repro were run.

## Next actions

1. Watch `stablyai/orca#2948` for merge, close without merge, stall, or incomplete fix.
2. Run secret-free current-main repro for `mini-swe-agent#826` in `corp-opensource-runner`; prove post-`#832` behavior and fill a regression card before any upstream action.
3. If mini-swe runner setup blocks, run `probe#568` CLI + Node SDK evidence capture for `symbols --format json` and record stdout/stderr/exit/JSON parse result.

## Active PR dashboard refresh, 2026-05-28 01:15 UTC

Bounded active-PR dashboard refresh over tracked upstream PRs: `OpenHands/software-agent-sdk#3394`, `anomalyco/opencode#29565/#29530`, `cline/cline#11087`, `CopilotKit/CopilotKit#5035`, `pydantic/pydantic-ai#5678/#5680`, and stale-clean `NousResearch/hermes-agent#15640`.

Parent live gates used `gh pr view` plus targeted adjacent PR checks for `pydantic#5605`, `opencode#29632`, and `cline#11075/#11071`. Six read-only roles and 3-role critique selected the same dashboard outcome.

`next_status: PR-OPEN / WATCH`

Upstream action count: `0`.

No upstream comment, rerun, rebase, force-push, close, or new adjacent PR was made.

Tracker comments: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4559896941), OpenHands [#22](https://github.com/serejaris/corp-opensource/issues/22#issuecomment-4559896966), opencode structured-output [#44](https://github.com/serejaris/corp-opensource/issues/44#issuecomment-4559896951), opencode webfetch [#23](https://github.com/serejaris/corp-opensource/issues/23#issuecomment-4559896968), Cline [#24](https://github.com/serejaris/corp-opensource/issues/24#issuecomment-4559896954), CopilotKit [#19](https://github.com/serejaris/corp-opensource/issues/19#issuecomment-4559896977).

| Lane | Live result | Next action |
|---|---|---|
| `OpenHands/software-agent-sdk#3394` | Open, non-draft, `MERGEABLE/BLOCKED`, reviewDecision still `CHANGES_REQUESTED`, status rollup empty; maintainer eval was already triggered and no visible eval/re-review is present. | Wait eval result / maintainer re-review. No repeat comment. |
| `opencode#29565` | Open, non-draft; duplicate/standards/compliance/contributor checks green; parent `gh` read returned mergeability `UNKNOWN/UNKNOWN`, so do not overclaim mergeability; no reviews. Adjacent structured-output PR `#29632` is open but not an exact cover. | Wait maintainer review; no new structured-output PR. |
| `opencode#29530` | Open, non-draft; duplicate/standards/compliance checks green; parent `gh` read returned mergeability `UNKNOWN/UNKNOWN`, so do not overclaim mergeability; guideline bot already satisfied. | Wait maintainer review. |
| `cline#11087` | Open, non-draft, `MERGEABLE/BLOCKED`; SDK quality, Ubuntu/Windows, and Socket checks green; Greptile gap already answered. Adjacent `#11075` and draft `#11071` increase provider-routing race risk. | Wait maintainer review; no new provider-routing work. |
| `CopilotKit#5035` | Open, non-draft, `MERGEABLE/BLOCKED`, `REVIEW_REQUIRED`; docs Vercel preview green; other Vercel contexts are team authorization failures, not proven code failures. | Wait maintainer review / Vercel team authorization. |
| `pydantic-ai#5678` | Open, non-draft, `MERGEABLE/CLEAN`; visible CI/harness/coverage green; no reviews. | Wait maintainer/bot review. |
| `pydantic-ai#5680` | Open, non-draft, `MERGEABLE/CLEAN`; visible CI/harness/coverage green; no reviews. Same-file race risk with open `#5605` in AG-UI adapter. | Wait review; no new UI adapter work. |
| `Hermes#15640` | Open, non-draft, `MERGEABLE/CLEAN`; no comments/reviews/checks; stale since `2026-04-25`. | Keep `PR-OPEN / WATCH-stale-clean`; no ping/close/rebase without fresh macOS validation or maintainer signal. |

3-role critique: factology found no approval/merge/close, actionable failing CI, exact supersede, merge conflict, or maintainer ask. Process gates allow only internal tracker/watch updates. Actionability remains monitor-only across all tracked PR lanes.

## Active PR dashboard refresh, 2026-05-28 01:54 UTC

Bounded active-PR dashboard refresh over tracked upstream PRs and duplicate-covered watch lanes: `OpenHands/software-agent-sdk#3394`, `anomalyco/opencode#29565/#29530`, `cline/cline#11087`, `CopilotKit/CopilotKit#5035`, `pydantic/pydantic-ai#5678/#5680`, stale-clean `NousResearch/hermes-agent#15640`, `stablyai/orca#2948`, and `colbymchenry/codegraph#345`.

Process: parent live `gh pr view` gates, 6 read-only roles in two waves, then 3-role critique. This is a monitor-only dashboard refresh, so runner repro was not required. Upstream action count: `0`.

No upstream comment, rerun, rebase, force-push, close, or new adjacent PR was made.

`next_status: WATCH`.

| Lane | Live result | Next action |
|---|---|---|
| `OpenHands/software-agent-sdk#3394` | Open, non-draft, `MERGEABLE/BLOCKED`, old `CHANGES_REQUESTED`, status rollup empty; maintainer eval/re-review still not visible. | Wait maintainer eval / re-review / workflow approval. No repeat comment. |
| `opencode#29565` | Open, non-draft; duplicate/standards/compliance/contributor checks green; current `gh` mergeability `UNKNOWN`; no maintainer review. | Wait maintainer review; no third opencode PR without distinct signal. |
| `opencode#29530` | Open, non-draft, `MERGEABLE/BLOCKED`; duplicate/standards/compliance/contributor-management checks green, but not full test CI; no maintainer review. | Wait maintainer review. |
| `cline#11087` | Open, non-draft, `MERGEABLE/BLOCKED`; visible SDK quality, Ubuntu/Windows Node 24 tests, and Socket checks green/skipped as expected; Greptile only commented, no approval. | Wait maintainer review; no new provider-routing work. |
| `CopilotKit#5035` | Open, non-draft, `MERGEABLE/BLOCKED`, `REVIEW_REQUIRED`; docs Vercel preview green; other Vercel contexts fail through team authorization URLs, not proven code failures. | Wait maintainer review / Vercel team authorization. |
| `pydantic-ai#5678` | Open, non-draft, `MERGEABLE/CLEAN`; broad CI/harness/coverage/smokeshow green; no approval. | Wait maintainer/bot review. |
| `pydantic-ai#5680` | Open, non-draft, `MERGEABLE/CLEAN`; broad CI/harness/coverage/smokeshow green; no approval. | Wait maintainer/bot review; no new UI adapter work. |
| `Hermes#15640` | Open, non-draft; current `gh` mergeability `UNKNOWN`; no comments/reviews/checks; stale since `2026-04-25`. | Keep `PR-OPEN / WATCH-stale-clean`; no ping/close/rebase without fresh macOS validation or maintainer signal. |
| `orca#2948` | Open upstream cover for `orca#2930`; `MERGEABLE/UNSTABLE`; only visible check is `track-community-pr` green; `#2930` remains open. | `WATCH / duplicate-covered`; re-enter only if `#2948` closes without merge, stalls with maintainer signal, or lands incomplete. |
| `codegraph#345` | Open exact-fix cover for `codegraph#507`, but now `CONFLICTING/DIRTY`, `REVIEW_REQUIRED`, no checks; `#507` remains open. | `WATCH / stale-conflicting coverage`; do not re-enter while `#345` is live unless maintainer asks, PR closes, or fresh repro proves an uncovered gap. |

6-role synthesis:

- Owned active PRs remain monitor-only; no approval/merge/close, actionable failing CI, maintainer ask, or superseding PR was found.
- `opencode#29530` can now be written as live `MERGEABLE/BLOCKED`, but its green checks are template/management checks, not a full test suite.
- `Hermes#15640` should not be described as currently `CLEAN/MERGEABLE`; latest `gh` reports mergeability `UNKNOWN`.
- `orca#2948` still blocks re-entry into `orca#2930`, but the live check set is light.
- `codegraph#345` still blocks re-entry into `#507` by exact-fix coverage, but its state is now stale/conflicting rather than clean coverage.

3-role critique:

- Factology: no contradiction blocks the internal `WATCH` update. Wording should say `codegraph#345` covers `#507` by exact fix, not that it explicitly closes it.
- Process: internal README/watch update is enough; tracker comments are unnecessary because no upstream-facing candidate status changed and no upstream action was taken.
- Actionability: no comment/rerun/rebase/ping/close/new PR is warranted. `codegraph#507` must not be re-entered while exact-cover PR `#345` remains live, even if conflicted.

## Active PR dashboard refresh, 2026-05-28 02:35 UTC

Bounded active-PR dashboard refresh over tracked upstream PRs: `OpenHands/software-agent-sdk#3394`, `anomalyco/opencode#29565/#29530`, `cline/cline#11087`, `CopilotKit/CopilotKit#5035`, `pydantic/pydantic-ai#5678/#5680`, and stale-clean `NousResearch/hermes-agent#15640`.

Process: parent live `gh pr view` gates, 6 read-only roles in two waves, then 3-role critique. This is a monitor-only dashboard refresh, so runner repro was not required. Required `open-source-pr-workflow` is unavailable here; this used documented fallback.

`next_status: WATCH`

Upstream action count: `0`.

No upstream comment, rerun, rebase, force-push, close, or new adjacent PR was made.

| Lane | Live result | Next action |
|---|---|---|
| `OpenHands/software-agent-sdk#3394` | Open, non-draft, `MERGEABLE/BLOCKED`, old `CHANGES_REQUESTED`, status rollup empty; maintainer eval/re-review still not visible. | Wait maintainer eval / re-review / workflow approval. No repeat comment. |
| `opencode#29565` | Open, non-draft, `MERGEABLE/BLOCKED`; duplicate/standards/compliance/contributor checks green; no review. Adjacent `#29632` changes structured-output retry/format flow and may interact semantically, but is not an exact cover. | Wait maintainer review; watch `#29632`. |
| `opencode#29530` | Open, non-draft, `MERGEABLE/BLOCKED`; duplicate/standards/compliance/contributor-management checks green, but not full test CI; no review. | Wait maintainer review. |
| `cline#11087` | Open, non-draft, `MERGEABLE/BLOCKED`; visible SDK quality, Ubuntu/Windows Node 24 tests, and Socket checks green; Greptile only commented, no approval. | Wait maintainer review; no new provider-routing work. |
| `CopilotKit#5035` | Open, non-draft, `MERGEABLE/BLOCKED`, `REVIEW_REQUIRED`; docs Vercel preview green; other Vercel contexts fail through team authorization URLs, not proven code failures. Release PR `#5069` is not a supersede, but can trigger published-package recheck if it merges first. | Wait maintainer review / Vercel team authorization; watch release timing. |
| `pydantic-ai#5678` | Open, non-draft, `MERGEABLE/CLEAN`; broad CI/harness/coverage/smokeshow green; no approval. | Wait maintainer/bot review. |
| `pydantic-ai#5680` | Open, non-draft, `MERGEABLE/CLEAN`; broad CI/harness/coverage/smokeshow green; no approval. Same-file overlap risk with open `#5602/#5605` in `pydantic_ai_slim/pydantic_ai/ui/ag_ui/_adapter.py`. | Wait maintainer/bot review; recheck if `#5602/#5605` merge first. |
| `Hermes#15640` | Open, non-draft, `MERGEABLE/CLEAN`; no comments/reviews/checks; stale since `2026-04-25`. Fresh `#29673` touches `hermes_cli/gateway.py` and gateway tests with launchd naming overlap, but is not a proven supersede. | Keep `PR-OPEN / WATCH-stale-clean`; no ping/close/rebase without fresh macOS validation or maintainer signal; watch `#29673`. |

6-role synthesis:

- No tracked PR was merged, closed, approved, superseded, or given a new maintainer ask.
- Live wording updates: `opencode#29565` and `opencode#29530` should be recorded as `MERGEABLE/BLOCKED`; `Hermes#15640` should be recorded as `MERGEABLE/CLEAN`.
- Main race watches: `opencode#29632` for structured-output semantics, `pydantic-ai#5602/#5605` for AG-UI adapter same-file overlap, `CopilotKit#5069` for release timing, and `Hermes#29673` for gateway/launchd naming overlap.
- One subagent drifted to `OpenHands/OpenHands#3394`; parent live gate and critique confirmed the canonical tracked PR is `OpenHands/software-agent-sdk#3394`.

3-role critique:

- Factology: no exact supersede or actionable failing CI found. `CopilotKit` Vercel failures are team authorization, not author-fixable. `Hermes#29673` is overlap risk, not a proven replacement.
- Process: internal dashboard update and one umbrella heartbeat are allowed; per-PR tracker comments and all upstream actions are unnecessary.
- Actionability: no comment, rerun, rebase, ping, close, or new PR is warranted. Continue monitor-only.

## Active PR dashboard refresh, 2026-05-28 03:10 UTC

Bounded active-PR dashboard refresh over tracked upstream PRs: `OpenHands/software-agent-sdk#3394`, `anomalyco/opencode#29565/#29530`, `cline/cline#11087`, `CopilotKit/CopilotKit#5035`, `pydantic/pydantic-ai#5678/#5680`, and stale `NousResearch/hermes-agent#15640`.

Process: parent live `gh pr view` gates, 6 read-only roles in two waves, then 3-role critique. This is a monitor-only dashboard refresh, so runner repro was not required. Upstream action count: `0`.

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560529774).

No upstream comment, rerun, rebase, force-push, close, or new adjacent PR was made.

`next_status: WATCH`

| Lane | Live result | Next action |
|---|---|---|
| `OpenHands/software-agent-sdk#3394` | Open, non-draft, `MERGEABLE/BLOCKED`, old `CHANGES_REQUESTED`; status rollup still empty; maintainer eval/re-review still not visible after our follow-up. | Wait maintainer eval / re-review / workflow approval. No repeat comment. |
| `opencode#29565` | Open, non-draft; current GitHub mergeability is `UNKNOWN/UNKNOWN`; duplicate/standards/compliance/contributor checks green; no review. Adjacent `#29632` is open but not an exact supersede. | Wait maintainer review; watch `#29632`. |
| `opencode#29530` | Open, non-draft; current GitHub mergeability is `UNKNOWN/UNKNOWN`; duplicate/standards/compliance/contributor-management checks green, but not full test CI; no review. | Wait maintainer review. |
| `cline#11087` | Open, non-draft, `MERGEABLE/BLOCKED`; visible SDK quality, Ubuntu/Windows Node 24 tests, and Socket checks green; Greptile gap already answered. Adjacent `#11075/#11071` are overlap risks, not exact supersedes. | Wait maintainer review; no new provider-routing work. |
| `CopilotKit#5035` | Open, non-draft, `MERGEABLE/BLOCKED`, `REVIEW_REQUIRED`; docs Vercel preview green; other Vercel contexts are team authorization failures, not proven code failures. Release PR `#5069` remains timing watch, not supersede. | Wait maintainer review / Vercel team authorization. |
| `pydantic-ai#5678` | Open, non-draft, `MERGEABLE/CLEAN`; broad CI/harness/coverage/smokeshow green; no approval. | Wait maintainer/bot review. |
| `pydantic-ai#5680` | Open, non-draft, `MERGEABLE/CLEAN`; broad CI/harness/coverage/smokeshow green; no approval. Same-file overlap risk with open `#5602/#5605`, but neither is an exact supersede. | Wait maintainer/bot review; recheck if `#5602/#5605` merge first. |
| `Hermes#15640` | Open, non-draft; current GitHub mergeability is `UNKNOWN/UNKNOWN`; no comments/reviews/checks; stale since `2026-04-25`. Fresh `#29673` is gateway naming overlap risk, not proven supersede. | Keep `PR-OPEN / WATCH-stale-unknown`; no ping/close/rebase without fresh macOS validation or maintainer signal; watch `#29673`. |

6-role synthesis:

- No tracked PR was merged, closed, approved, superseded, or given a new maintainer ask.
- Factology correction: do not describe `opencode#29565/#29530` as `MERGEABLE/BLOCKED` or `Hermes#15640` as `MERGEABLE/CLEAN`; current `gh` reports `UNKNOWN/UNKNOWN` for all three.
- `UNKNOWN/UNKNOWN` alone is not a rebase/close/ping trigger; no conflict or actionable maintainer request is proven.
- Adjacent PRs remain watch-only: `opencode#29632`, `cline#11075/#11071`, `CopilotKit#5069`, `pydantic-ai#5602/#5605`, and `Hermes#29673`.

3-role critique:

- Factology: avoid stale mergeability overclaims; keep pydantic wording as clean/green but unapproved, not done.
- Process: internal dashboard update and one umbrella heartbeat are enough; per-PR tracker comments would be noise.
- Actionability: no comment, rerun, rebase, ping, close, or new PR is warranted. Continue monitor-only.

## Repo-universe candidate refresh, 2026-05-28 01:45 UTC

Bounded Paperclip-like / AI-native scouting refresh over browser/control-plane, terminal-agent and MCP/SDK repos.

The required `open-source-bug-scouting` skill is unavailable in this environment. Attempted 6-agent cycle stopped at 3 read-only scouting roles because the multi-agent tool returned `agent thread limit reached`; parent completed live GitHub gates and then ran the required 3-role critique before status updates.

`next_status: CANDIDATE / WATCH`

Upstream action count: `0`.

No upstream comment, PR, ping, rerun, or rebase was made.

Tracker comments: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4559945459), BrowserOS [#81](https://github.com/serejaris/corp-opensource/issues/81#issuecomment-4559945472), browser-agent scope [#70](https://github.com/serejaris/corp-opensource/issues/70#issuecomment-4559945473), MCP ecosystem [#75](https://github.com/serejaris/corp-opensource/issues/75#issuecomment-4559945467), high-churn terminal scope [#74](https://github.com/serejaris/corp-opensource/issues/74#issuecomment-4559945493), AI SDK scope [#66](https://github.com/serejaris/corp-opensource/issues/66#issuecomment-4559945499), FastMCP [#55](https://github.com/serejaris/corp-opensource/issues/55#issuecomment-4559945503), Emdash tracker [#83](https://github.com/serejaris/corp-opensource/issues/83).

| Lane | Live result | Decision |
|---|---|---|
| `browseros-ai/BrowserOS#1005` | Open/unassigned; reporter's 2026-05-26 update shows repeated identical tool re-registration; exact PR search found no cover. | Keep `CANDIDATE`; runner fail-before still required. |
| `browser-use/browser-use#4846` | Open bug, but critique found live overlap with `#4881/#4847` in MCP/CDP lifecycle area. | `WATCH / duplicate-overlap`; no PR. |
| `modelcontextprotocol/inspector#1368` | Open/unassigned V2 UI-state issue; exact PR search clean. | `CANDIDATE`; blocked on UI/state repro and test card. |
| `google-gemini/gemini-cli#27431` | Open MCP discovery failure with concrete `/mcp reload` error; no exact PR found; Google CLA/help-wanted process applies. | `CANDIDATE`; likely `COMMENT-FIRST` only after repro/diagnosis. |
| `generalaction/emdash#1875` | Open/unassigned; maintainer says PR welcome; exact PR search clean. | `CANDIDATE` via [#83](https://github.com/serejaris/corp-opensource/issues/83) / [watch](emdash-1875-linux-safestorage.md); runner Linux Secret Service repro required. |
| `vercel/ai#15652` | Open/no comments; no exact PR found; adjacent older structured-output PR exists but does not cover Anthropic `disableParallelToolUse` override. | `CANDIDATE`; Node/pnpm runner fixture required. |
| `vercel/ai#15613` | Open/no comments; relevant Vertex provider-executed `code_execution` round-trip bug, but broader fixture/provider churn. | `WATCH` until repro. |
| `PrefectHQ/fastmcp#4154` | Open; prior `#4156` closed after maintainer asked for reusable result-transforming abstraction, not a one-off wrapper. | `WATCH`; design/assignment-first. |
| `charmbracelet/crush#3024` | Open; maintainer/reporter discussion active, and open retry PRs `#1611/#2998` create overlap. | `WATCH`; no comment/PR without mock-provider evidence. |

3-role critique: factology confirmed no lane is closed or exact-covered except the noted overlap risks; process gates require runner/current-main repro for all candidate lanes and assignment/comment-first where applicable; actionability found no `PR-READY` lane and selected upstream action count `0`.

## Candidate readiness refresh, 2026-05-28 01:08 UTC

Bounded readiness refresh over the five strongest current candidate lanes: `generalaction/emdash#1875`, `browseros-ai/BrowserOS#1005`, `modelcontextprotocol/inspector#1368`, `google-gemini/gemini-cli#27431`, and `vercel/ai#15652`.

Process: parent live GitHub gates, runner/tooling gate, 6 read-only scouting roles in two waves, then 3-role critique.

Runner/tooling gate: `corp-server` does not resolve from this environment; issue [#10](https://github.com/serejaris/corp-opensource/issues/10) still says no live runner provisioned. Local environment has Node `v20.19.2`, no `pnpm`, and no `bun`, so no candidate can move to `PR-READY` here.

`next_status: CANDIDATE`

Upstream action count: `0`.

No upstream comment, PR, ping, rerun, or rebase was made.

Tracker comments: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4559994596), Emdash [#83](https://github.com/serejaris/corp-opensource/issues/83#issuecomment-4559994603), BrowserOS [#81](https://github.com/serejaris/corp-opensource/issues/81#issuecomment-4559994630), MCP ecosystem [#75](https://github.com/serejaris/corp-opensource/issues/75#issuecomment-4559994633), high-churn terminal [#74](https://github.com/serejaris/corp-opensource/issues/74#issuecomment-4559994654), AI SDK [#66](https://github.com/serejaris/corp-opensource/issues/66#issuecomment-4559994624).

| Lane | Live result | Decision |
|---|---|---|
| `generalaction/emdash#1875` | Open/unassigned; maintainer-contributor PR-welcome signal remains; no open exact PR. Parent verified closed unmerged prior-art PRs `#1907/#1908/#1909/#1910/#1911` for the same D-Bus / `gnome-libsecret` approach. | Keep `CANDIDATE`, but with prior-art risk. Future work must avoid closed-PR failures: build break, no `execFileSync` timeout, fragile `includes("true")`, KDE/Plasma detection, and incomplete user `--password-store` override handling. |
| `browseros-ai/BrowserOS#1005` | Open/unassigned; reporter's 2026-05-26 controlled repro remains latest signal; exact open PR search found no repeated `notifications/tools/list_changed` flood/dedupe fix. | Keep `CANDIDATE`; current-main MCP runtime/log card required. |
| `modelcontextprotocol/inspector#1368` | Open/unassigned with `v2` label; exact PR search clean, but adjacent V2/disconnect/OAuth/session/list_changed churn exists. | Keep `CANDIDATE`; V2 UI/state repro and focused test card required. |
| `google-gemini/gemini-cli#27431` | Open/unassigned; reporter supplied `/mcp reload` `MCP error -32000: Connection closed`; no exact PR. Adjacent MCP PRs `#27383/#25937` touch discovery/failure behavior. | Keep `CANDIDATE`; practical gate is repro first, then `COMMENT-FIRST` if the gap is generic and not covered by adjacent PRs. |
| `vercel/ai#15652` | Open/unassigned/no comments; no exact PR for Anthropic `jsonTool` forcing `disableParallelToolUse`; adjacent provider/structured-output PRs `#10274/#15112/#10812/#12903` remain watch risks. | Keep `CANDIDATE`; Node/pnpm package fixture, changeset/signed-commit readiness, and fresh adjacent PR search required. |

6-role synthesis: all five issues are still open; no exact open PR was found. `emdash#1875` is still the best runner target because the process signal is strongest and Linux Secret Service repro is decisive. If the runner lacks DBus/Electron support, `vercel/ai#15652` is the fallback because its repro can be a focused package fixture.

3-role critique: no lane is `PR-READY`; allowed actions are internal tracker/watch updates only. `emdash#1875` remains an allowed `CANDIDATE`, but the closed PRs are now mandatory prior-art/design evidence before any future PR.

## Candidate lane correction refresh, 2026-05-28 01:14 UTC

Bounded refresh over six current lanes: `SWE-agent/mini-swe-agent#826`, `ag-ui-protocol/ag-ui#1635`, `mastra-ai/mastra#17118`, `probelabs/probe#568`, `generalaction/emdash#1875`, and `browseros-ai/BrowserOS#1005`.

Process: parent live GitHub gates, runner/tooling gate, 6 read-only scouting roles in two waves, then 3-role critique before tracker/watch updates. The required `open-source-bug-scouting` skill is not installed in this environment, so this is a documented fallback. Upstream action count: `0`.

Runner/tooling gate: `corp-server` still does not resolve, tracker [#10](https://github.com/serejaris/corp-opensource/issues/10) still says no live `corp-opensource-runner` is provisioned, and local tooling is insufficient for these lanes: Node `v20.19.2`, no `pnpm`, no `bun`, no `rustc/cargo`.

Tracker comments: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560052525), mini-swe [#53](https://github.com/serejaris/corp-opensource/issues/53#issuecomment-4560052531), Probe [#80](https://github.com/serejaris/corp-opensource/issues/80#issuecomment-4560052533), AG-UI [#56](https://github.com/serejaris/corp-opensource/issues/56#issuecomment-4560053754), Mastra [#58](https://github.com/serejaris/corp-opensource/issues/58#issuecomment-4560053752), BrowserOS [#81](https://github.com/serejaris/corp-opensource/issues/81#issuecomment-4560053769), Emdash [#83](https://github.com/serejaris/corp-opensource/issues/83#issuecomment-4560053766).

| Lane | Live correction | Decision |
|---|---|---|
| `SWE-agent/mini-swe-agent#826` | Issue is open/no comments, but merged upstream PR `#832` is a stronger potential cover than prior notes implied: it is timeout-related and changed agent wall-clock handling, even though it did not touch `LocalEnvironment.execute()`. | `WATCH`; first runner target, but only for post-`#832` current-main re-test. No upstream comment/PR until PID/`ps` fail-before card proves a remaining process-tree leak. |
| `ag-ui-protocol/ag-ui#1635` | Issue open/no comments/no exact PR. `#1727` is adjacent output-processor work, not an exact payload-less chunk fix; broader Mastra adapter PRs keep collision risk medium. | `CANDIDATE`; future upstream mode is comment-first only after runner-backed `@ag-ui/mastra` test evidence and assignment/code-owner gate. |
| `mastra-ai/mastra#17118` | Issue open/labeled; maintainer asked for MRE. Merged `#17054` is a relevant partial precedent for wrapping `ask_user` picker option labels and must be checked before claiming a clean gap; open `#17083` remains broad Mastracode overlap. | `CANDIDATE`; no `PR-READY` until terminal-width MRE/regression card proves slash-command description wrapping is still uncovered. |
| `probelabs/probe#568` | Issue open with `bug` + `external`; upstream bot/maintainer comment is a positive bug-shape signal and also asks for exact command/API/env consistency details. | `CANDIDATE`; compact fallback runner target. Do not answer upstream until CLI + Node SDK stdout/stderr/exit/JSON-parse evidence is captured. |
| `generalaction/emdash#1875` | Issue open/unassigned; PR-welcome signal remains; no exact open PR. Closed prior-art PRs `#1907/#1908/#1909/#1910/#1911` remain mandatory design evidence. | `CANDIDATE`; second runner target if Linux DBus/Secret Service/Electron tooling is available. |
| `browseros-ai/BrowserOS#1005` | Issue open/unassigned with strong reporter logs; no exact PR found. Plausible path remains repeated MCP server/tool registration, but broadcast source is not runner-proven. | `CANDIDATE`; requires Bun/MCP runtime card on current main. |

6-role synthesis: five lanes remain internal candidates, while `mini-swe#826` is demoted to stricter `WATCH` because `#832` may have already fixed enough of the timeout symptom to make any upstream action noisy without re-test. Best operational target after runner provisioning is `mini-swe#826`; fallback is `probe#568`; `emdash#1875` is next if runner has Linux Secret Service / Electron support.

3-role critique: factology required corrections for `mini-swe#832`, Mastra `#17054`, and the semantics of the Probe `#568` upstream response. Process gates forbid `PR-READY` and forbid upstream comments in this cycle because no runner-backed evidence exists. Actionability allows internal tracker/watch updates only.

## Candidate backlog refresh, 2026-05-28 02:19 UTC

Bounded refresh over six active candidate/watch lanes: `oraios/serena#1519`, `generalaction/emdash#1875`, `browseros-ai/BrowserOS#1005`, `probelabs/probe#568`, `mastra-ai/mastra#17118`, and `ag-ui-protocol/ag-ui#1635`.

Detailed note: [candidate backlog refresh](candidate-backlog-refresh-2026-05-28-0219.md).

Process: parent live GitHub gates, runner/tooling gate, 6 read-only scouting roles in two waves, then 3-role critique. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are unavailable here, so this was the documented fallback.

`next_status: CANDIDATE`

Upstream action count: `0`.

No upstream comment, PR, ping, rerun, or rebase was made.

Runner/tooling gate remains closed: `corp-server` does not resolve, `corp-opensource-runner` is not provisioned per `#10`, local Node is `v20.19.2`, and local env lacks `pnpm`, `bun`, Docker, `rustc/cargo`, and the required runner matrix.

| Lane | Live correction | Decision |
|---|---|---|
| `probelabs/probe#568` | Open, `bug` + `external`, unassigned; upstream confirms legitimate bug shape but asks for exact CLI/API/env details. Open PRs `#561/#264` are unrelated. Source review says root cause is still unproven because `symbols.rs` looks JSON-clean while `Pattern:` / `Path:` output is in `main.rs::handle_search`. | Primary `CANDIDATE` / first runner target. |
| `oraios/serena#1519` | Open, unassigned, no labels/comments; no open PR cover for dashboard host-port 403. Source still maps to `src/serena/dashboard.py` Host validation. | Reserve `CANDIDATE`; needs Docker/port-mapping repro and security-preserving Flask test. |
| `browseros-ai/BrowserOS#1005` | Open, unassigned; reporter's 2026-05-26 update gives current root-cause signal around full tool re-registration on every MCP call/focus/connect; no exact PR cover. | Reserve `CANDIDATE`; needs Bun/MCP repro, AGPL/CLA gate, and a notification-dedupe design that does not break lifecycle/security. |
| `generalaction/emdash#1875` | Open, unassigned, PR-welcome signal remains; no open cover, but closed exact PR history `#1907-#1911` is mandatory prior art. | `WATCH / CANDIDATE-risk`; no PR without avoiding closed-PR failure modes. |
| `mastra-ai/mastra#17118` | Open, team labels and MRE request; no exact PR cover, but dense Mastracode queue and `#17054` partial wrapping precedent. | `COMMENT-FIRST / WATCH` after MRE; not primary. |
| `ag-ui-protocol/ag-ui#1635` | Open, unassigned; no exact cover, but broad stale PR `#1253` strongly overlaps Mastra adapter/chunk mapping and is `CONFLICTING` / `CHANGES_REQUESTED`. | `WATCH`; no PR without test evidence and positioning against `#1253`. |

3-role critique: `CANDIDATE` is justified by uncovered Probe/Serena/BrowserOS lanes, but no lane is `PR-READY`. Create no new tracker issue. Update umbrella `#52` and primary Probe tracker `#80`; avoid noisy comments on unchanged reserve lanes.

## Candidate/watch backlog refresh, 2026-05-28 03:16 UTC

Bounded refresh over active candidate/watch lanes plus the new CUA/UI automation runner target: `probelabs/probe#568`, `web-infra-dev/midscene#2544`, `oraios/serena#1519`, `browseros-ai/BrowserOS#1005`, `generalaction/emdash#1875`, `mastra-ai/mastra#17118`, and `ag-ui-protocol/ag-ui#1635`.

Process: parent live GitHub gates, duplicate/PR search, 6 read-only roles in two waves, then internal status critique. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are unavailable here, so this was the documented fallback.

`next_status: CANDIDATE`

Upstream action count: `0`.

No upstream comment, PR, ping, rerun, or rebase was made.

Tracker comments: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560558645), Probe [#80](https://github.com/serejaris/corp-opensource/issues/80#issuecomment-4560558654), CUA/UI automation [#72](https://github.com/serejaris/corp-opensource/issues/72#issuecomment-4560558620).

Runner gate remains closed: `corp-opensource-runner` is not available from this environment, and no lane has current-main fail-before evidence. Therefore no lane is `PR-READY`; even `COMMENT-FIRST` lanes must wait for runner evidence if the comment would change upstream status.

| Lane | Live result | Decision |
|---|---|---|
| `probelabs/probe#568` | Open, labels `bug` + `external`, unassigned; upstream comment confirms bug shape but asks for exact command/API/env details. No exact open PR found; historical JSON-output PRs and `#546` are context only. | Primary `CANDIDATE` / first runner target. Minimal contract: capture CLI/Node SDK stdout, stderr, exit code and JSON parse failure on current main; expected fix keeps machine JSON clean on stdout and sends progress/debug elsewhere. |
| `web-infra-dev/midscene#2544` | Open, unassigned; no exact PR for web `longPress` 600ms cap. Nearby `#2387` and `#1801` are historical feature/rename context, not a cover. | `WATCH` but second practical runner target. Promote only after current-main timing evidence proves requested duration above 600ms is capped/truncated and a unit/integration contract is feasible. |
| `oraios/serena#1519` | Open, unassigned, no labels/comments; no exact PR for Docker host-port dashboard 403. Dashboard/Docker-adjacent PRs are not direct cover. | Reserve `CANDIDATE`; Docker/port-mapping runner repro required, and any fix must preserve Host/DNS-rebinding protection. |
| `browseros-ai/BrowserOS#1005` | Open; reporter's 2026-05-26 update remains strong evidence, but fresh MCP-adjacent merge `#1064` means runtime surface may have shifted. No exact list_changed dedupe PR found. | `WATCH` until repeat MCP/event-stream repro on current main; good later candidate if runner supports browser/MCP runtime. |
| `generalaction/emdash#1875` | Open and PR-welcome signal remains, but closed exact prior-art PRs `#1907/#1908/#1909/#1910/#1911` are mandatory. | `WATCH / prior-art gated`; no PR evidence or upstream action before reading closed-PR failure modes and maintainer preference. |
| `mastra-ai/mastra#17118` | Open with `bug`, `mastracode`, `impact:medium`, `effort:medium`; no exact PR found, but merged `#17054/#17005` and open `#16550/#17083/#17006` make TUI/wrapping overlap high. | `WATCH`; can become `COMMENT-FIRST` only after terminal-width MRE proves slash-command description wrapping remains uncovered. |
| `ag-ui-protocol/ag-ui#1635` | Open; no exact issue-closing PR, but Mastra adapter overlap is high, especially broad `#1253` plus `#1727/#1687/#1556/#1615`. | `COMMENT-FIRST` after runner-backed SSE/chunk fixture and positioning against `#1253`; not PR-ready. |

6-role synthesis: `probe#568` is still the best low-infra Paperclip/Codex-style target because it is a structured-output contract bug in an agent/code-search tool. `midscene#2544` is the strongest second runner target because UI action timing is directly relevant to CUA reliability and no exact PR is visible. `serena#1519` remains a valid reserve candidate, while `BrowserOS#1005` is relevant but heavier; `emdash#1875`, `mastra#17118`, and `ag-ui#1635` are blocked by prior-art, MRE, or overlap gates.

Critique: final cycle status can be `CANDIDATE` because Probe and Serena are open, uncovered and actionable once runner evidence exists. It cannot be `PR-READY` because no fail-before repro was run. Upstream action count remains `0`.

## Fresh browser/CUA/terminal discovery cycle 36, 2026-05-28 03:26 UTC

Detailed note: [cycle 36 browser/CUA/terminal](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-36-browser-cua-terminal.md).

Process: GitHub repo search, parent live repo/issue/PR gates, 6 read-only roles in two waves, then internal critique. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are unavailable here, so this was the documented fallback.

`next_status: CANDIDATE`

Upstream action count: `0`.

No upstream comment, PR, ping, rerun, or rebase was made.

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560610697).

Selected future runner target: `simular-ai/Agent-S#195`. It is open, unassigned, no exact PR was found, and the bug has a small deterministic CUA action contract: `WindowsACI.hotkey("enter")` should produce a single `enter` key token, not split it into `e`, `n`, `t`, `e`, `r`. It is not `PR-READY` because current-main repro/test evidence is still missing.

Reserve watch lanes: `nanobrowser#270`, `magnitudedev/browser-agent#154`, and `browserwing#18`. Duplicate-covered/no-go lanes: `nanobrowser#291 -> #318`, `Agent-S#196 -> #197`, `ntegrals/openbrowser` issue-disabled PR backlog, and `browserwing#11/#12` security lanes.

Factology corrections recorded: one subagent confused `ntegrals/openbrowser` with `openbrowser/openbrowser`; parent live gates control. Another subagent drifted from `earendil-works/pi` into unrelated `pydantic/pydantic-ai` numbering; parent gates control and `pi#5055/#5065/#5095` remain open/assigned `WATCH`.

## Fresh Continue link lead, 2026-05-28 05:55 UTC

Detailed note: [Continue #12507 context provider link](continue-12507-context-provider-link.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561177594).

Process: parent live GitHub/source gates, 6-role fallback scouting in staged groups, then focused 3-role critique. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are unavailable here, so this was the documented fallback.

`next_status: LEAD`

Upstream action count: `0`.

Runner action count: `0`.

Selected lead: `continuedev/continue#12507`. The issue is open, unassigned, labeled `area:chat`, `area:context-providers`, `area:docs`, and `kind:bug`. Current `main` still has the stale `Add more context providers` docs URL in `gui/src/components/mainInput/TipTapEditor/utils/getSuggestion.ts`; exact open PR cover was not found. Broad `#12202` is docs-file-only and not a cover for this UI source link.

Critique result: factology and duplicate gates pass, process gates pass for `LEAD`, but actionability/strategy keeps this below the existing runner backlog. It is a useful opportunistic UI/docs-link lead, not a Paperclip-like runtime candidate and not `PR-READY`. Do not reorder primary runner targets: `probelabs/probe#568`, `google-gemini/gemini-cli#27503`, `vercel/ai#15652`, and `langchain4j/langchain4j#5313` remain ahead.

## Fresh Vercel docs lead, 2026-05-28 06:10 UTC

Detailed note: [Vercel AI #15663 onFinish/onEnd docs](vercel-ai-15663-onfinish-onend-docs.md).

Tracker comments: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561245208), Vercel tracker [#86](https://github.com/serejaris/corp-opensource/issues/86#issuecomment-4561246297).

Process: parent live GitHub/source gates, duplicate/PR search, 6-role fallback scouting in staged groups. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are unavailable here, so this was the documented fallback.

`next_status: LEAD`

Upstream action count: `0`.

Runner action count: `0`.

Selected lead: `vercel/ai#15663`. The issue is open, unassigned, unlabeled, and had no comments at gate time. Current `main` still contains `streamText({ ... onFinish ... })` examples in docs/cookbook/provider MDX files after merged `#15245` introduced `onEnd` and deprecated `onFinish` for the final lifecycle callback. The issue's scope caveat is valid: `toUIMessageStreamResponse({ onFinish })` is a different callback surface and should not be swept.

Duplicate/race result: no exact open PR cover. `#14772` is path-adjacent for `content/cookbook/01-next/73-mcp-tools.mdx` but does not cover `onFinish -> onEnd`; `#15245` is merged source/regression context, not a cover.

Critique result: factology and duplicate gates pass, but this stays `LEAD` because it is docs-only, lower priority than runtime/tool-call candidates, and has Vercel license/process friction. No upstream comment/PR. Primary runner backlog remains Probe, Gemini, Vercel `#15652`, and LangChain4j.

## Vercel AI #15663 follow-up, 2026-05-28 06:26 UTC

Detailed note: [Vercel AI #15663 onFinish/onEnd docs](vercel-ai-15663-onfinish-onend-docs.md).

Process: live gate refresh plus 6-role fallback scouting refresh. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are unavailable here, so this remains the documented fallback. No upstream comment/PR.

`next_status: LEAD/WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Delta: `vercel/ai#15663` remains open, unassigned, and unlabeled, but the original "comments absent" gate is now stale. The issue has 3 comments from author `n-satoshi061`; the author offered to open a PR and expanded the affected surface to include `packages/mcp/README.md`, several `examples/ai-functions/src/**` files that still demonstrate `onFinish`, and filename-level `on-finish` examples that may need an `on-end` rename.

Duplicate/race result: no exact open PR cover found for `#15663`, `onFinish -> onEnd`, `examples/ai-functions`, or the author. Race risk increased anyway because the issue author has effectively claimed the fix lane. Keep as `LEAD/WATCH`, not `PR-READY`, and do not open a competing upstream PR unless maintainer direction or author inactivity changes the lane.

## Fresh Codex Desktop WSL bwrap candidate, 2026-05-28 06:32 UTC

Detailed note: [OpenAI Codex #24873 Desktop WSL bwrap sandbox](openai-codex-24873-desktop-wsl-bwrap.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561378941).

Process: fresh GitHub discovery, parent live issue/repo/duplicate gates, 6-role fallback scouting, then focused 3-role critique. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are unavailable here, so this remains the documented fallback. No upstream comment/PR.

`next_status: CANDIDATE`

Upstream action count: `0`.

Runner action count: `0`.

Selected lead: `openai/codex#24873`. The issue is open, unassigned, unlabeled, with no comments at gate time. It reports Codex Desktop WSL `codex-cli 0.133.0` failing sandboxed tool execution before a trivial command runs because the WSL binary cannot find system `bwrap` and has no adjacent bundled `codex-resources/bwrap`; `use_legacy_landlock = true` then fails with a permission-profile panic.

Duplicate/race result: no exact open PR or issue cover found. Closed `#21915` is relevant prior art but was for the VS Code extension / remote Linux surface and does not cover Desktop WSL cache layout or the Landlock fallback panic.

Critique result: factology and duplicate gates pass for `CANDIDATE`, but not `PR-READY`. This needs Windows host + WSL2 + Codex Desktop repro, `RUST_BACKTRACE=1`, launcher/resource lookup evidence, and a contribution/process gate. Current `corp-opensource-runner` remains unavailable via `#10`, and a Linux-only runner would not close the Desktop WSL packaging gap.

## Fresh Claude Code Agent Teams TUI lead, 2026-05-28 06:41 UTC

Detailed note: [Claude Code #63019 Agent Teams permission TUI corruption](claude-code-63019-agent-teams-permission-tui.md).

Tracker comments: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561433388), high-churn terminal/runner [#74](https://github.com/serejaris/corp-opensource/issues/74#issuecomment-4561434267).

Process: fresh GitHub discovery, parent live issue/repo/duplicate gates, 6-role fallback scouting, then focused 3-role critique. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are unavailable here, so this remains the documented fallback. No upstream comment/PR.

`next_status: LEAD`

Upstream action count: `0`.

Runner action count: `0`.

Selected lead: `anthropics/claude-code#63019`. The issue is open, unassigned, labeled `bug`, `has repro`, `platform:macos`, `area:tui`, `area:agents`, and `area:permissions`, with no comments at gate time. It reports terminal output corruption when a permission prompt appears while the user is navigating the expanded Agent Teams selection UI on Claude Code `2.1.153`; screenshots and steps are provided.

Duplicate/race result: no exact open PR cover found. Nearby issues `#49865` and `#8618` are related prior art but not exact cover: `#49865` is a closed Agent Teams permission crash path, while `#8618` is broad older terminal rendering corruption without this Agent Teams permission prompt trigger.

Critique result: valid fresh lead, but not `CANDIDATE`/`PR-READY`. Repro needs macOS/TUI evidence and likely manual or upstream-compatible terminal-buffer harness; the repo appears issue-first/no top-level `CONTRIBUTING`, and a comment without new evidence would be noise. Core runner backlog remains Probe, Gemini, Vercel `#15652`, LangChain4j, then Codex WSL-specific follow-up.

## Fresh Nanobot candidate pair, 2026-05-28 06:53 UTC

Detailed note: [Nanobot #4040/#4039 unified session and context budget](nanobot-4040-4039-unified-session-context-budget.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561501637).

Process: fresh GitHub discovery, parent live issue/repo/duplicate gates, 6-role fallback scouting, then focused 3-role critique. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are unavailable here, so this remains the documented fallback. No upstream comment/PR.

`next_status: CANDIDATE`

Upstream action count: `0`.

Runner action count: `0`.

Selected candidate pair: `HKUDS/nanobot#4040` and `HKUDS/nanobot#4039`. Both are open, unassigned, unlabeled, and had no comments at gate time. `#4040` reports that `/stop` can fail to cancel an active task when `unified_session` is enabled because cancellation appears to use the raw session key rather than the effective unified session key. `#4039` reports that context snipping may count tool definitions during the initial estimate but fail to reserve tool-schema tokens when computing retained-history budget.

Duplicate/race result: no exact open PR or duplicate issue found. This is distinct from cycle 34 Nanobot lanes `#4006/#4013`, which were duplicate-covered by PRs `#4011/#4020`.

Critique result: `#4040` is the stronger runtime interrupt/cancellation candidate; `#4039` is a lower-confidence but strategically relevant context/tool-schema budget candidate. No upstream comment/PR because issue bodies already include root-cause hypotheses and suggested fixes; useful action requires runner/source-backed repro. Fresh lower-priority leads `oh-my-claudecode#3163` and `github/gh-aw#35423` remain unpromoted.

## Fresh runtime batch, 2026-05-28 07:08 UTC

Detailed note: [Fresh runtime batch 2026-05-28 07:08 UTC](fresh-runtime-batch-2026-05-28-0708.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561586615).

Process: fresh GitHub discovery, parent live issue/repo/duplicate gates, 6-role fallback scouting, then focused 3-role critique. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are unavailable here, so this remains the documented fallback. No upstream comment/PR.

`next_status: CANDIDATE`

Upstream action count: `0`.

Runner action count: `0`.

Selected candidate: `NousResearch/hermes-agent#33712`. It is open, unassigned, unlabeled, with no comments at gate time. `hermes dashboard` fails on NixOS/container with `ModuleNotFoundError: No module named 'hermes_cli.dashboard_auth'`. Exact duplicate/PR cover was not found; adjacent PR `#33311` touches `dashboard_auth` but fixes reverse-proxy login URL prefixes, not a missing packaged module.

Watch lanes: `openai/codex#24879` is material but covered-risk due same-title closed `#24877`, prior `#21928`, and active auto-review model override PR `#23767`; `anthropics/claude-code#63025` is high-severity Desktop/SSH history-index loss but needs Windows Desktop + SSH Remote evidence; `NVIDIA/TensorRT-LLM#14676` is valid AutoDeploy/NVRTC watch but GPU/CI-heavy and likely maintainer-internal.

## Fresh Candle correctness candidate, 2026-05-28 07:21 UTC

Detailed note: [Candle #3567 Qwen2 mask and autograd watch](candle-3567-qwen2-mask-and-autograd-watch.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561668867).

Process: fresh GitHub discovery, parent live issue/repo/duplicate gates, 6-role fallback scouting, then focused 3-role critique. Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are unavailable here, so this remains the documented fallback. No upstream comment/PR.

`next_status: CANDIDATE`

Upstream action count: `0`.

Runner action count: `0`.

Selected candidate: `huggingface/candle#3567`. It is open, unassigned, unlabeled, with no comments at gate time. It reports that Qwen2 `prepare_attention_mask` becomes bidirectional when `attn_mask` is provided because it lacks a causal triangle, causing silent wrong outputs in batched eval/training. No exact PR cover was found.

Watch lanes: `candle#3568/#3569` are valid autograd no-bwd reports around `rope` and `softmax_last_dim`, but they are coordination-sensitive because open PR `#3526` already addresses the same `apply_op*_no_bwd` family for RmsNorm `#2168`. Keep them as `WATCH / coordination-first`, not cold PR targets.

## Hermes and Claude live delta, 2026-05-28 07:34 UTC

Detailed note: [Fresh runtime batch 2026-05-28 07:08 UTC](fresh-runtime-batch-2026-05-28-0708.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561756753).

`next_status: CANDIDATE`

Upstream action count: `0`.

Runner action count: `0`.

Material delta: `NousResearch/hermes-agent#33712` was triaged upstream with labels `type/bug`, `comp/cli`, `area/nix`, and `P3`. It remains open, unassigned, with no comments and no exact PR cover. This confirms the bug/CLI/Nix shape but does not make it `PR-READY`; useful action still requires Nix/container package-surface repro.

Claude side watch: `anthropics/claude-code#63028` is a fresh Web/plugin `has repro` lead, `#63025` gained a third-party diagnostic comment showing the same SSH Remote UI symptom with intact `projects`, and `#63029` is a weaker Web archive-restore data-loss report with duplicate/process risk. No upstream comments or PRs.

## CLIProxyAPI Claude tool-name candidate, 2026-05-28 07:45 UTC

Detailed note: [CLIProxyAPI #3592 Claude tool name reverse map](cliproxyapi-3592-claude-tool-name-reverse-map.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561820934).

`next_status: CANDIDATE`

Upstream action count: `0`.

Runner action count: `0`.

Fresh discovery после `07:35Z` выбрал `router-for-me/CLIProxyAPI#3592`. Repo популярный (`35,153` stars, `5,823` forks, not archived). Issue описывает компактный Claude Code / Anthropic Messages proxy contract bug: incoming tool schema advertises `Glob`, но streamed response возвращает `tool_use.name: "glob"`, из-за чего Claude Code case-sensitive dispatcher отклоняет tool. В report есть request evidence, response SSE evidence, packet-capture narrowing и suggested tests.

Duplicate/race: open PR list содержит adjacent request-side/Bifrost tool remapping PR `#3214`, но exact response-side per-request reverse-map cover не найден. GitHub Search API вернул `403` rate-limit во время broader duplicate search, поэтому перед upstream action нужен повторный exact search.

## Post-CLIProxy duplicate refresh and opencode spill watch, 2026-05-28 07:55 UTC

Detailed notes:

- [CLIProxyAPI #3592 Claude tool name reverse map](cliproxyapi-3592-claude-tool-name-reverse-map.md)
- [opencode #29694 tool-output spill cleanup](opencode-29694-tool-output-spill-cleanup.md)

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561885928).

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Material delta: duplicate/race refresh нашёл `router-for-me/CLIProxyAPI#2746`, open non-draft but conflicting PR, который implements per-request collision-aware `forwardMap`/`reverseMap` for OAuth tool-name remapping и включает SSE stream reverse coverage. Это достаточно близко к `#3592`, поэтому CLIProxyAPI downgraded с clean `CANDIDATE` до `WATCH / duplicate-covered-risk` до settle `#2746`.

Fresh watch: `anomalyco/opencode#29694` reports `~/.local/share/opencode/tool-output` spill files growing to `63G`; issue указывает на existing 7-day cleanup function в `packages/opencode/src/tool/truncate.ts`, но visible caller не найден. Exact open PR cover не найден, но issue уже assigned to `nexxeln`, поэтому keep as high-value assigned `WATCH`, not a cold PR lane.

## Assigned opencode IME/TUI watch and duplicate refresh, 2026-05-28 08:05 UTC

Detailed notes:

- [opencode #29697 IME TUI stale cells watch](opencode-29697-ime-tui-stale-cells-watch.md)
- [opencode #29694 tool-output spill cleanup](opencode-29694-tool-output-spill-cleanup.md)
- [CLIProxyAPI #3592 Claude tool name reverse map](cliproxyapi-3592-claude-tool-name-reverse-map.md)

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561938155).

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Process: parent live GitHub gates, focused duplicate/PR search, 6-role reused-agent scouting, then 3-role critique before tracker/watch updates. The required `open-source-bug-scouting` skill is not installed in this environment, so this is a documented fallback.

Fresh watch: `anomalyco/opencode#29697` reports macOS Terminal.app CJK IME composition corrupting the TUI layout, with stale cells after commit. Exact duplicate/PR cover was not found in focused searches, but the issue is already assigned to `simonklee` and the repro needs macOS Terminal.app + IME behavior. Keep as `WATCH`, not a cold PR lane.

Existing high-value watch: `anomalyco/opencode#29694` remains open and assigned to `nexxeln`, with no material post-`07:55Z` delta and no exact cleanup PR cover found. It remains the more practical opencode source-level lane, but assignment and unavailable runner keep it out of `PR-READY`.

Duplicate/race correction: `router-for-me/CLIProxyAPI#2746` is still a strong cover-risk for `#3592`, but the latest critique notes it may not fully cover the TitleCase client-tool response-restore case if `#2746` only reverses actual request-side renames. Keep `#3592` as `WATCH / duplicate-covered-risk`, not `NO-GO`; re-entry after `#2746` settles or a current-main retest proves `Glob -> glob` still leaks.

Other live gates: `anthropics/claude-code#63034` remains upstream-owned `WATCH` because duplicate bot linked `#62459/#59843` and the author disputed duplicate rather than asking for an external patch. `openai/codex#24881` is `NO-GO / duplicate-design-watch` for this cycle because stronger prior path-glob skill issue `#19671` already exists. No upstream comment or PR is allowed from this cycle.

## Claude Code agent/MCP fresh watch, 2026-05-28 08:12 UTC

Detailed note: [Claude Code #63042/#63041 agent and MCP watch](claude-code-63042-63041-agent-mcp-watch.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561973121).

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Process: parent live GitHub gates, focused duplicate/PR search, 6-role reused-agent scouting, then synthesis critique before tracker/watch updates. The required `open-source-bug-scouting` skill is not installed in this environment, so this is a documented fallback.

Material fresh signals after the `08:02Z` tracker heartbeat:

- `anthropics/claude-code#63042` reports stale local git identity leaking across Agent tool worktree dispatches, breaking signed-commit verification and multi-agent attribution. Focused search found no exact PR cover; related `#51596` and `#36981` are adjacent, not exact.
- `anthropics/claude-code#63041` reports `claude mcp get <server>` printing secret-bearing MCP env vars in cleartext. Focused search found adjacent prior issues `#44888` and `#58850`, but no exact active cover for this command/output slice.
- `anthropics/claude-code#63031` is a secondary watch for `claude agents` workflow slash-command completion missing from the dispatch input, adjacent to fixed `#61424`.

Fresh opencode `#29698` is a weak `LEAD/WATCH`: macOS Desktop appears to confuse projects with the same folder name but different absolute paths, but the report has no steps/screenshots and may overlap `#29692`. No status promotion.

Existing active lanes had no material post-cutoff transition. `vercel/ai#15652`, `probelabs/probe#568`, and `google-gemini/gemini-cli#27503` remain the top runner-backed candidate backlog, but runner `#10` is still unavailable and no PR-ready gate is closed.

Decision: keep this cycle at `WATCH`. Claude Code items are product-owned from our contribution perspective; no upstream comment/PR without a future explicit maintainer ask or new source-backed evidence.

## Covered PR refresh: Pydantic/Vercel/Codex, 2026-05-28 08:16 UTC

Detailed note: [Pydantic/Vercel/Codex covered refresh 2026-05-28 08:16](pydantic-vercel-codex-covered-refresh-2026-05-28-0816.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562007431).

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Process: parent live GitHub gates, focused duplicate/PR search, 6-role reused-agent scouting, then synthesis critique before tracker/watch updates. The required `open-source-bug-scouting` skill is not installed in this environment, so this is a documented fallback.

Material deltas after the `08:08Z` tracker heartbeat:

- `pydantic/pydantic-ai#5663` is now owner/author-covered by draft PR `#5670` (`CONFLICTING`, author `colesmcintosh`, assignee `dsfaccini`). The issue remains open and pydanty marks the model-name/profile/docs update PR-ready, but this is not our candidate lane.
- `vercel/ai#15664` has exact targeted PR `#15665` by issue author `onmax`, open non-draft, `MERGEABLE`, review required. No competing PR.
- `openai/codex#24475` got a fresh confirming comment about Codex subagent tool hangs/reconnect symptoms on version `134`. It remains a strong product/app-server watch with labels `bug`, `CLI`, `app`, `connectivity`, `subagent`, `app-server`; no OSS patch lane or runner-backed source path exists here.

Other active lanes had no material post-cutoff transition. The operational runner backlog remains `vercel/ai#15652`, `probelabs/probe#568`, and `google-gemini/gemini-cli#27503`, but runner `#10` is still unavailable.

Decision: `WATCH`. No upstream comment or PR.

## Claude peers MCP auto-summary candidate, 2026-05-28 08:25 UTC

Detailed note: [claude-peers-mcp #64 gpt-5.4 max_completion_tokens](claude-peers-mcp-64-gpt54-max-completion-tokens.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562086486).

`next_status: CANDIDATE`

Upstream action count: `0`.

Runner action count: `0`.

Process: parent live GitHub gates, focused duplicate/PR search, source read-only gate, 6-role reused-agent scouting, then synthesis critique before tracker/watch updates. The required `open-source-bug-scouting` skill is not installed in this environment, so this is a documented fallback.

Selected candidate: `louislva/claude-peers-mcp#64`. Repo is MIT, not archived, `2,040` stars / `271` forks. Issue reports auto-summary silently failing because `shared/summarize.ts` sends `max_tokens` to `gpt-5.4-nano`; GitHub source gate confirms the current request body has `model: "gpt-5.4-nano"` plus `max_tokens: 100`, and non-OK responses return `null` without logging.

Duplicate/race: focused issue search found only `#64`; focused PR search found no exact `max_tokens -> max_completion_tokens` cover. Race risk is still real because the repo has an active PR queue and the reporter offered to send a PR. No upstream comment/PR before repeat duplicate search, contribution/process check, and runner-backed validation.

Other fresh signal: `anthropics/claude-code#63045` is a good product-owned TUI regression watch for diff SGR background bleed in `2.1.150-2.1.153`, but Claude Code has no OSS patch lane here. Existing `claude-code#63043` now has duplicate-bot links `#62659/#31548/#58918`, so keep it at `WATCH`.

## Codex/Claude product watch, 2026-05-28 08:30 UTC

Detailed note: [Codex/Claude product watch 2026-05-28 08:30](codex-claude-product-watch-2026-05-28-0830.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562138734).

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Process: parent live GitHub gates, focused duplicate/PR search, 6-role reused-agent scouting, then synthesis critique before tracker/watch updates. The required `open-source-bug-scouting` skill is not installed in this environment, so this is a documented fallback.

Fresh product-watch deltas:

- `openai/codex#24884` reports Windows Codex App WSL mode only works with `danger-full-access`; `sandbox = "elevated"` hides distros and `sandbox = "unelevated"` hits `Wsl/EnumerateDistros/Service/E_ACCESSDENIED`. Adjacent broader cover-risk is `#21470`, but no exact PR cover was found.
- `anthropics/claude-code#63047` re-files Desktop/Cowork plugin `PostToolUse` hooks silently skipping, with six weeks of production evidence. It is related to open canonical `#16288` and closed/stale `#51904/#27398/#51281`, so duplicate/process risk is high.

Candidate carry-forward: `louislva/claude-peers-mcp#64` remains open with no PR cover or post-cutoff state change. Keep it as internal `CANDIDATE-needs-runner-validation`, not upstream action.

Other active lanes had no material post-cutoff transition. Operational runner backlog remains `vercel/ai#15652`, `probelabs/probe#568`, and `google-gemini/gemini-cli#27503`; runner `#10` is still unavailable.

Decision: `WATCH`. No upstream comment or PR.

## Claude/opencode follow-up watch, 2026-05-28 08:35 UTC

Detailed note: [Claude/opencode follow-up watch 2026-05-28 08:35](claude-opencode-followup-watch-2026-05-28-0835.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562194681).

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Process: parent live GitHub gates, focused duplicate/PR search, reused 6-role scouting synthesis from the current cycle, then focused 3-role critique before tracker/watch updates. The required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this is a documented fallback.

Material deltas after the `08:30Z` product watch:

- `anomalyco/opencode#29699` is now `MERGED` at live parent gate (`updatedAt 2026-05-28T08:28:02Z`), closing the upstream-owned websocket timeout cover lane for local implementation.
- `anthropics/claude-code#63048` opened as a product-owned `/code-review` skill bug: empty `@{upstream}...HEAD` can silently fall back to `main...HEAD` on long-lived branches and review other people's commits; same report asks for human-readable non-`--comment` output instead of raw JSON-only findings.

Duplicate/PR gate: focused issue search for `code-review fallback main HEAD JSON` found `#63048` plus broad adjacent skills/core issues, but no exact duplicate; focused PR search returned no PR cover.

Decision: `WATCH`. `#63048` is useful product intel, but there is no external patch lane here. `claude-peers-mcp#64` remains the only current small source-level `CANDIDATE-needs-runner-validation`.

## Fresh AI-native watch, 2026-05-28 08:42 UTC

Detailed note: [Fresh AI-native watch 2026-05-28 08:42](fresh-ai-native-watch-2026-05-28-0842.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562244160).

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Process: parent live GitHub gates, focused duplicate/PR search, reused 6-role scouting synthesis, then focused 3-role critique before tracker/watch updates. The required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this is a documented fallback.

Material deltas after the `08:36Z` tracker heartbeat:

- `openai/codex#24884` got an external contributor intent comment proposing a WSL-bootstrap-only sandbox exception. This raises race risk and keeps the lane `WATCH`.
- `anthropics/claude-code#63050` is a fresh Monitor tool bug with `has repro`: Monitor commands do not appear to source the same shell snapshot as Bash, so PATH-dependent tools fail on macOS/Nix. Exact PR search returned no cover, but Claude Code is product-owned here.
- `router-for-me/CLIProxyAPI#3594` is a fresh `LEAD` for Amp Neo CLI WebSocket/Rivet passthrough. Exact PR search returned no cover, but protocol/session complexity blocks candidate promotion.
- `google-gemini/gemini-cli#27517` is a weak `WATCH` for `ioctl(2) failed, EBADF` in node-pty resize under VS Code/xterm.js; repro is not yet strong enough.
- `anomalyco/opencode#29703` remains `NO-GO/WATCH`: compliance-gated, duplicate-flagged against `#27822/#23249`, and assigned.

Carry-forward: `claude-peers-mcp#64` remains the only current small source-level `CANDIDATE-needs-runner-validation`; no post-08:36 state change and no exact PR cover.

Decision: `WATCH`. No upstream comment or PR.

## Fresh AI-native watch, 2026-05-28 08:48 UTC

Detailed note: [Fresh AI-native watch 2026-05-28 08:48](fresh-ai-native-watch-2026-05-28-0848.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562290239).

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Process: parent live GitHub gates, focused duplicate/PR search, reused 6-role scouting synthesis, then focused 3-role critique before tracker/watch updates. The required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this is a documented fallback.

Material deltas after the `08:42Z` watch:

- `google-gemini/gemini-cli#27517` gained duplicate-bot `status/possible-duplicate` with a broad possible-duplicate list; keep `WATCH / NO-GO for now`.
- `anomalyco/opencode#29704` opened for OpenCode Desktop animation GPU spikes on older Intel iGPU, but it is assigned to `Hona` and duplicate-gated against `#21939`; keep `WATCH`.
- `mem0ai/mem0#5284` is a fresh external PR, non-draft `MERGEABLE/REVIEW_REQUIRED`, with three regression tests for LangChain vector store `None` scores; already covered.
- `ag-ui-protocol/ag-ui#1801` is a fresh Kotlin Interrupts types enhancement with author PR intent; watch for duplicate/race, no cold PR.
- `anthropics/claude-code#62959` and `openai/codex#24457` received external workaround/analysis comments, but both remain product watch lanes.

Carry-forward: `claude-peers-mcp#64` remains the best compact source-level `CANDIDATE-needs-runner-validation`; `CLIProxyAPI#3594` remains an uncovered but protocol-heavy `LEAD`.

Decision: `WATCH`. No upstream comment or PR.

## Opencode/Codex watch, 2026-05-28 08:55 UTC

Detailed note: [Opencode/Codex watch 2026-05-28 08:55](opencode-codex-watch-2026-05-28-0855.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562334053).

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Process: parent live GitHub gates, focused duplicate/PR search, reused 6-role scouting synthesis, then focused 3-role critique before tracker/watch updates. The required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this is a documented fallback.

Material deltas after the `08:48Z` watch:

- `anomalyco/opencode#29704` closed at `08:51:45Z` after the reporter agreed `#21939` will resolve the GPU impact too. Status becomes `NO-GO / duplicate-covered`.
- `anomalyco/opencode#28150` got a fresh comment linking open PR `#28689` as the granular read permission deny fix. PR `#28689` is open, non-draft, `MERGEABLE`; keep `WATCH / covered-by-PR`.
- `anomalyco/opencode#16962` got fresh Warp-to-Mac-mini confirmation while iTerm2 succeeds, but it is assigned and duplicate-linked to `#15907/#6586/#8237`; keep `WATCH`.
- `openai/codex#24884` contributor comment was edited to say the public repo may not contain the Windows App WSL bootstrap path; this reinforces `WATCH / product-private-surface`.

Carry-forward: `claude-peers-mcp#64` remains the best compact source-level `CANDIDATE-needs-runner-validation`; `CLIProxyAPI#3594` remains an uncovered but protocol-heavy `LEAD`.

Decision: `WATCH`. No upstream comment or PR.

## Gemini/opencode covered watch, 2026-05-28 09:02 UTC

Detailed note: [Gemini/opencode covered watch 2026-05-28 09:02](gemini-opencode-covered-watch-2026-05-28-0902.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562394501).

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Process: parent live GitHub gates, focused duplicate/PR search, reused 6-role scouting synthesis, then focused 3-role critique before tracker/watch updates. The required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this is a documented fallback.

Material deltas after the `08:55Z` watch:

- `google-gemini/gemini-cli#27518` opened as another EBADF `resizePty` crash report, but it is already duplicate-bot labeled and belongs to the active EBADF cluster (`#27510/#26433/#27501/#27499/#27517`); keep `WATCH / NO-GO`.
- `anomalyco/opencode#29706` opened as a generic Desktop server-500 report, assigned to `jlongster`, `needs:compliance`, and possible duplicate `#29083`; keep `WATCH / NO-GO`.
- `modelcontextprotocol/python-sdk#2705` is a fresh external PR for streamable-HTTP/SSE test port races, non-draft `MERGEABLE/REVIEW_REQUIRED`; already covered.
- `anomalyco/opencode#29705` is a fresh external PR for snapshot pathspec resolution from git subdirectories, non-draft `MERGEABLE`, compliance satisfied; already covered.
- `anthropics/claude-code#63051` is product-transient API `529 Overloaded`; no OSS patch lane.

Carry-forward: `claude-peers-mcp#64` remains the best compact source-level `CANDIDATE-needs-runner-validation`; `CLIProxyAPI#3594` remains an uncovered but protocol-heavy `LEAD`.

Decision: `WATCH`. No upstream comment or PR.

## Claude/MCP/opencode watch, 2026-05-28 09:08 UTC

Detailed note: [Claude/MCP/opencode watch 2026-05-28 09:08](claude-mcp-opencode-watch-2026-05-28-0908.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562442210).

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Process: parent live GitHub gates, focused duplicate/PR search, reused 6-role scouting synthesis, then focused 3-role critique before tracker/watch updates. The required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this is a documented fallback.

Material deltas after the `09:02Z` watch:

- `anthropics/claude-code#63051` gained duplicate/product labels and duplicate bot links `#62188/#39763/#41628`; keep `NO-GO / duplicate-product-transient`.
- `anthropics/claude-code#63052` opened as a long-input TUI truncation report on macOS/Ghostty, already duplicate-labeled; keep product `WATCH`.
- `modelcontextprotocol/python-sdk#2695` got a follow-up proposal for additional `convert_result=True` return-shape tests after `#2700`; coordination-first watch.
- `anomalyco/opencode#26625` got a fresh stable-vs-PR-branch `/timestamps` comment; issue remains assigned and PR-linked, so keep `WATCH / PR-linked ambiguity`.
- `vercel/ai#15667` is a docs-only OpenRouter link PR, already covered.
- `JetBrains/koog#2079` is a fresh `maxTokens` type/semantics question; keep low-priority `LEAD/WATCH` until clarified.

Carry-forward: `claude-peers-mcp#64` remains the best compact source-level `CANDIDATE-needs-runner-validation`; `CLIProxyAPI#3594` remains an uncovered but protocol-heavy `LEAD`.

Decision: `WATCH`. No upstream comment or PR.

## opencode/Claude/MCP/LangChain watch, 2026-05-28 09:26 UTC

Detailed note: [opencode/Claude/MCP/LangChain watch 2026-05-28 09:26](opencode-claude-mcp-langchain-watch-2026-05-28-0926.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562611188).

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Process: parent live GitHub gates, repo-local issue/PR lists after GitHub Search API rate-limit, reused 6-role scouting synthesis, then focused 3-role critique before tracker/watch updates. The required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this is a documented fallback.

Material deltas after the `09:16Z` watch:

- `sst/opencode#29709` merged at `2026-05-28T09:23:47Z`; keep `WATCH / settled`.
- `sst/opencode#29711` is a fresh assigned CJK paste placeholder issue, duplicate-hinted against `#17032/#29707/#25854`; keep `WATCH / duplicate-cluster`.
- `sst/opencode#29712` closes `#29711`, is open/non-draft/mergeable, and converts display-width paste extmark offsets to JS char indices; keep `WATCH / covered-by-PR`.
- `sst/opencode#29710` remains open/non-draft/mergeable covering adjacent `#29707`.
- `sst/opencode#29708` got reporter clarification differentiating it from `#27907`, but remains assigned and duplicate-risk; keep `LEAD/WATCH`.
- `anthropics/claude-code#63054` is a fresh OSC 52/tmux clipboard regression with WSL/devcontainer repro; keep product `WATCH` because broad duplicate/PR search was rate-limited and public patch surface is unclear.
- `modelcontextprotocol/typescript-sdk#2163` covers `#2162` invalid tool argument protocol errors; keep `WATCH / covered-by-PR`.
- `langchain-ai/langchain#37723` has a fresh root-cause comment and PR intent; keep `WATCH / contributor-intent-race`.

Carry-forward: `claude-peers-mcp#64` remains the best compact source-level `CANDIDATE-needs-runner-validation`; runner `#10` is unavailable, so no repro/fix promotion.

Decision: `WATCH`. No upstream comment or PR.

## Claude/opencode/AG-UI watch, 2026-05-28 09:16 UTC

Detailed note: [Claude/opencode/AG-UI watch 2026-05-28 09:16](claude-opencode-agui-watch-2026-05-28-0916.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562501539).

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Process: parent live GitHub gates, focused duplicate/PR search, reused 6-role scouting synthesis, then focused 3-role critique before tracker/watch updates. The required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this is a documented fallback.

Material deltas after the `09:08Z` watch:

- `anomalyco/opencode#29707` is now covered by open PR `#29710`, which fixes wide-character summarized paste corruption with a regression test; keep `WATCH / covered-by-PR`.
- `ag-ui-protocol/ag-ui#1802` covers `#1801` for Kotlin SDK Interrupts types/serialization; keep `WATCH / covered-by-PR`.
- `anomalyco/opencode#29709` is a maintainer-owned ACP first-session startup PR; keep `WATCH / already-covered`.
- `anomalyco/opencode#29708` is assigned and duplicate-hinted against `#28112/#27907`; keep `WATCH`.
- `anthropics/claude-code#63053` is a product-owned VS Code markdown rendering enhancement; keep `WATCH`.
- `anthropics/claude-code#63052` is duplicate-labeled; keep `NO-GO / duplicate`.

Carry-forward: `claude-peers-mcp#64` remains the best compact source-level `CANDIDATE-needs-runner-validation`; `CLIProxyAPI#3594` remains an uncovered but protocol-heavy `LEAD`.

Decision: `WATCH`. No upstream comment or PR.

## Claude/opencode post-cutoff watch, 2026-05-28 08:20 UTC

Detailed note: [Claude/opencode post-cutoff watch 2026-05-28 08:20](claude-opencode-postcutoff-watch-2026-05-28-0820.md).

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562042732).

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Process: parent live GitHub gates, focused duplicate/PR search, 6-role reused-agent scouting, then synthesis critique before tracker/watch updates. The required `open-source-bug-scouting` skill is not installed in this environment, so this is a documented fallback.

Material deltas after the `08:13Z` tracker heartbeat:

- `anthropics/claude-code#63043` is a strong Windows tool-process leak watch: `rg.exe` plus `conhost.exe` accumulate across long sessions, with measured `~2000` processes and `~14 GB` RAM. Exact search found no PR cover; adjacent orphan-process issues are not exact.
- `anthropics/claude-code#63044` is a weaker stats/TUI timezone watch: dashboard peak hour differs from local JSONL aggregation by one hour.
- `anomalyco/opencode#29699` is an upstream-owned open PR for OpenAI websocket response timeout behavior, non-draft and `MERGEABLE`; no competing work.
- `anomalyco/opencode#29692` cleared compliance after reporter added Desktop repro for `erp1`/`erp2` folder confusion, but it is assigned to `Hona`, so no cold PR/comment.

Other active lanes had no material post-cutoff transition. Operational runner backlog remains `vercel/ai#15652`, `probelabs/probe#568`, and `google-gemini/gemini-cli#27503`; runner `#10` is still unavailable.

Decision: `WATCH`. No upstream comment or PR.
