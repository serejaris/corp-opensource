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
