# Upstream follow-up, 2026-05-28

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Affected internal trackers: [#82](https://github.com/serejaris/corp-opensource/issues/82), [#81](https://github.com/serejaris/corp-opensource/issues/81), [#80](https://github.com/serejaris/corp-opensource/issues/80), [#58](https://github.com/serejaris/corp-opensource/issues/58), [#56](https://github.com/serejaris/corp-opensource/issues/56), [#53](https://github.com/serejaris/corp-opensource/issues/53).

Tracker comments: [#82 demotion](https://github.com/serejaris/corp-opensource/issues/82#issuecomment-4559803855), [#52 synthesis](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4559804608), [#53 mini-swe refresh](https://github.com/serejaris/corp-opensource/issues/53#issuecomment-4559839933), [#52 mini-swe synthesis](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4559840785).

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this follow-up used the documented fallback: parent live GitHub gates, 6 read-only subagents, then a 3-role critique.

No upstream PR/comment/ping was made.

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
