# Upstream Follow-up 2026-05-27

## Check 21:13 UTC

Bounded active-PR dashboard refresh with 6 read-only subagents and parent live gates via `gh pr view`. No upstream PR/comment/rerun/rebase/close action was taken. Final dashboard `next_status`: `WATCH`; item-level open PRs remain `PR-OPEN / monitor-only` or stale backlog.

| Upstream | Current state | Action |
|---|---|---|
| `OpenHands/software-agent-sdk#3394` | Open, non-draft; `mergeStateStatus=BLOCKED`, `reviewDecision=CHANGES_REQUESTED`; check rollup empty. Maintainer previously said it looks fine and triggered eval, but no visible eval pass or re-review is present. | `PR-OPEN / WATCH`. Wait maintainer eval, re-review, or workflow approval. No repeat comment. |
| `anomalyco/opencode#29565/#29530` | Both open, non-draft; `mergeStateStatus=BLOCKED`; visible duplicate/standards/compliance checks are green. No maintainer review yet. | `PR-OPEN / WATCH`. Wait maintainer review; no third opencode PR without distinct signal. |
| `cline/cline#11087` | Open, non-draft; `mergeStateStatus=BLOCKED`; visible SDK quality, Ubuntu/Windows Node 24 tests and Socket checks are green. Greptile only commented, and the follow-up already addressed that gap. | `PR-OPEN / monitor only`. Wait maintainer review. |
| `CopilotKit/CopilotKit#5035` | Open, non-draft; `mergeStateStatus=BLOCKED`, review required. Vercel docs preview is green; other Vercel contexts fail on team authorization URLs and are not a proven code failure. Claude review is disabled for the fork unless maintainers trigger it. | `WATCH`. Wait maintainer review/team authorization. |
| `pydantic-ai#5678/#5680` | Both open, non-draft, `MERGEABLE`, `mergeStateStatus=CLEAN`; broad CI/harness/coverage/smokeshow checks are green. No maintainer review yet. | `PR-OPEN / monitor only`. Wait maintainer/bot review or merge. |
| `NousResearch/hermes-agent#15640` | Open, non-draft, `MERGEABLE`, `mergeStateStatus=CLEAN`; stale since `2026-04-25`, no comments/reviews/checks. | Keep in stale cleanup backlog. Do not ping/rebase/close in this dashboard refresh; use a separate cleanup cycle if needed. |

6-subagent synthesis / critique:

- Factology: none of the tracked PRs is merged, approved, or ready-to-merge. For blocked PRs, record the process/review gate instead of saying only "mergeable".
- Process: upstream comment, CI rerun, rebase, force-push, close, or replacement PR is not justified. This is a monitor-only dashboard heartbeat.
- Verification: local tests/runner are not needed because no new code change or fixable code-check failure exists. GitHub checks are sufficient for dashboard state; OpenHands and Hermes lack visible CI/eval evidence, but their blockers are upstream process/staleness.
- Actionability: final status `WATCH`; the only non-monitor item is `hermes-agent#15640`, which remains a separate stale cleanup decision.

## Check 20:48 UTC

Bounded active-PR dashboard refresh with 6 read-only subagents, parent live gates via `gh pr view` / `gh issue view`, and 3-subagent critique. No upstream PR/comment/rerun/rebase/close action was taken. Final dashboard `next_status`: `WATCH`; item-level open PRs remain `PR-OPEN / monitor-only`.

| Upstream | Current state | Action |
|---|---|---|
| `OpenHands/software-agent-sdk#3394` | Open, `MERGEABLE`; old bot `CHANGES_REQUESTED` remains and merge state is blocked. Maintainer said it looks fine and triggered eval; PR-visible check rollup is still empty and no eval pass is visible. | `PR-OPEN / WATCH`. Wait maintainer eval, re-review, or workflow approval. |
| `anomalyco/opencode#29565/#29530` | Both open, non-draft and `MERGEABLE`. Visible duplicate/standards/compliance checks are green; no maintainer review yet. | `PR-OPEN / WATCH`. Wait maintainer review; no third opencode PR without distinct signal. |
| `cline/cline#11087` | Open, `MERGEABLE`; visible quality, Ubuntu/Windows SDK tests and Socket checks green after follow-up `e1ecc63a1`; Greptile only commented; no approval. | `PR-OPEN / monitor only`. Wait maintainer review. |
| `CopilotKit/CopilotKit#5035` | Open, `MERGEABLE`, review required. Vercel docs preview is green; other Vercel contexts still require team authorization and are not a proven code failure. | `WATCH`. Wait maintainer review/team authorization. |
| `pydantic-ai#5678/#5680` | Both open, `MERGEABLE`, merge-state clean, visible CI/coverage/harness/checks green; no maintainer approval yet. | `PR-OPEN / monitor only`. Wait maintainer/bot review or merge. |
| `NousResearch/hermes-agent#15640` | Open, `MERGEABLE`, but stale since 2026-04-25 with no comments/reviews/checks. Outside the current Hermes/Codex outage flow. | Keep in stale cleanup backlog; no ping/close/rebase in this monitoring cycle. |
| `continuedev/continue#12334` | Issue still open; no new comments after our current-main evidence comment. Exact failing autocomplete path is still unknown. | `WATCH / needs reporter path`. No PR without a failing current-main path. |
| `modelcontextprotocol/typescript-sdk#2115/#2116/#2138` | Issue still open. Exact competing PRs `#2116` and `#2138` are both open, `MERGEABLE`, review required, with green checks, tests, and changesets. | `WATCH / duplicate-race`. No duplicate PR; wait maintainer choice/merge/close. |

3-subagent critique:

- factology: do not say `approved`, `ready`, `ready to merge`, `merged` or `accepted`. `MERGEABLE` plus green checks is only technical state, not maintainer approval.
- process: upstream comment, rerun, rebase, close, or new PR is not justified. This is a monitor-only update; runner/repro is not needed because no new `PR-READY` candidate exists.
- actionability: update internal dashboard/watch wording only; final status `WATCH`.

## Check 20:30 UTC

Bounded active-PR dashboard refresh with 6 read-only subagents, parent live gates via `gh pr view` / `gh issue view`, and 3-subagent critique. One subagent output drifted into the prior Morph SDK cycle and was excluded from the active-PR synthesis. No upstream PR/comment/rerun/rebase/close action was taken. Final dashboard `next_status`: `WATCH`; item-level open PRs remain `PR-OPEN / monitor-only`.

| Upstream | Current state | Action |
|---|---|---|
| `OpenHands/software-agent-sdk#3394` | Open, `MERGEABLE`; old bot `CHANGES_REQUESTED` remains. Maintainer said it looks fine and triggered eval; PR-visible check rollup is still empty and no eval pass is visible. | `PR-OPEN / WATCH`. Wait maintainer eval, re-review, or workflow approval. |
| `anomalyco/opencode#29565/#29530` | Both open and non-draft. Visible duplicate/standards/compliance checks are green; current `gh pr view` reports mergeability `UNKNOWN`; no maintainer review yet. | `PR-OPEN / WATCH`. Wait maintainer review; no third opencode PR without distinct signal. |
| `cline/cline#11087` | Open, `MERGEABLE`; visible quality, Ubuntu/Windows SDK tests and Socket checks green after follow-up `e1ecc63a1`; Greptile only commented; no approval. | `PR-OPEN / monitor only`. Wait maintainer review. |
| `CopilotKit/CopilotKit#5035` | Open, review required; current `gh pr view` reports mergeability `UNKNOWN`. Vercel docs preview is green; other Vercel contexts require team authorization and are not a proven code failure. | `WATCH`. Wait maintainer review/team authorization. |
| `pydantic-ai#5678/#5680` | Both open, `MERGEABLE`, visible CI/coverage/harness/checks green; Codex review quota warning only. | `PR-OPEN / monitor only`. Wait maintainer/bot review or merge. |
| `NousResearch/hermes-agent#15640` | Open, `MERGEABLE`, clean, but stale since 2026-04-25 with no comments/reviews/checks. Outside the current Hermes/Codex outage flow. | Move to stale cleanup backlog; no ping/close in this monitoring cycle. |
| `continuedev/continue#12334` | Issue still open; no new comments after our current-main evidence comment. Exact failing autocomplete path is still unknown. | `WATCH / needs reporter path`. No PR without a failing current-main path. |
| `modelcontextprotocol/typescript-sdk#2115/#2116/#2138` | Issue still open. Exact competing PRs `#2116` and `#2138` are both open, `MERGEABLE`, review required, with green checks, tests, and changesets. | `WATCH / duplicate-race`. No duplicate PR; wait maintainer choice/merge/close. |
| `aaif-goose/goose#9447` | Closed, not merged. Maintainer said fixed in `#9449`. | Keep historical/duplicate-race lesson only. |

3-subagent critique:

- factology: do not say `approved`, `merged`, `accepted`, or `ready to merge` unless GitHub state proves it. Use `UNKNOWN` mergeability where current `gh` returned it for opencode/CopilotKit.
- process: upstream comment, rerun, rebase, close, or new PR is not justified. This is a monitor-only update; runner/repro is not needed because no new `PR-READY` candidate exists.
- actionability: update internal dashboard/watch wording only; final status `WATCH`.

## Check 19:33 UTC

Bounded active-PR dashboard refresh with 6 read-only scouting subagents, parent live gates via `gh pr view`, and 3-subagent critique. No upstream PR/comment/rerun/close action was taken. Final dashboard `next_status`: `WATCH`; item-level `PR-OPEN` remains monitor-only for still-open PRs.

| Upstream | Current state | Action |
|---|---|---|
| `aaif-goose/goose#9447` | Closed, not merged. Maintainer comment says fixed in `#9449`; historical `changes` CI failure is no longer actionable. | Move out of active PRs into closed/superseded lessons. No upstream action. |
| `OpenHands/software-agent-sdk#3394` | Open, `MERGEABLE`, merge-state blocked; old `CHANGES_REQUESTED` still active. Maintainer said it looks fine but wants eval; eval was triggered, check rollup is empty in `gh pr view`. | `PR-OPEN / WATCH`. Wait eval/re-review/workflow approval. |
| `anomalyco/opencode#29565/#29530` | Both open and non-draft. Visible duplicate/standards/compliance checks are green; merge-state remains blocked and there is no maintainer review yet. | `PR-OPEN / WATCH`. Wait maintainer review; no third opencode PR without a distinct signal. |
| `cline/cline#11087` | Open, `MERGEABLE`, merge-state blocked. Visible quality/SDK Ubuntu+Windows/Socket checks are green after follow-up `e1ecc63a1`; no approval yet. | `PR-OPEN / monitor only`. Wait maintainer review. |
| `CopilotKit/CopilotKit#5035` | Open, `MERGEABLE`, review required. Vercel docs preview is available; other Vercel previews require team authorization and are not a proven code failure. | `WATCH`. Wait maintainer review/team authorization. |
| `pydantic-ai#5678/#5680` | Both open, `MERGEABLE`, merge-state clean, visible CI/coverage/harness checks green; Codex review quota warning only. | `PR-OPEN / monitor only`. Wait maintainer review/merge. |
| `NousResearch/hermes-agent#15640` | Open and stale since 2026-04-25; no comments/reviews/assignee. This old macOS launchd/Login Item fix is outside the current Hermes/Codex outage flow. | Keep as stale backlog cleanup decision: ping/rebase, close as abandoned, or remove from active dashboard later. |

3-subagent critique:

- factology: do not say `goose#9447` merged or accepted; correct wording is superseded/fixed by `#9449`. Do not say opencode/cline are approved or ready to merge while merge-state is blocked.
- process: no upstream comment, rerun, close, or new PR is justified from this monitoring cycle.
- actionability: update dashboard/watch/tracker only; no lane becomes `PR-READY`.

## Check 18:20 UTC

Bounded follow-up with 6 subagents over active PRs, comment-first/watch lanes, duplicate-race lanes, repo-candidate queue, tracker consistency, and process gates. Parent live gates used `gh pr view` / `gh issue view`; GitHub Search API for `SWE-agent/mini-swe-agent#826` is still rate-limited. 3-subagent critique chose final `next_status: WATCH`. No upstream PR/comment opened. Tracker comments: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4557412645), `mini-swe-agent#826` lane [#53](https://github.com/serejaris/corp-opensource/issues/53#issuecomment-4557410943).

| Upstream | Current state | Action |
|---|---|---|
| `aaif-goose/goose#9447` | Open, `MERGEABLE`, review required, merge-state blocked. `changes` still failed from GitHub diff API transient; `check-quarantined` and `machete` green. | `WATCH`. Wait maintainer rerun/review; no duplicate CI comment. |
| `OpenHands/software-agent-sdk#3394` | Open, `MERGEABLE`, merge-state blocked; GitHub still reports old `CHANGES_REQUESTED`; check rollup empty. Maintainer eval/re-review remains the gate. | `WATCH`. Wait eval or maintainer review. |
| `opencode#29565/#29530` | Both open; duplicate/standards/compliance checks green; no maintainer review yet. Parent `gh` returned mergeability for `#29565` inconsistently across checks, so do not overclaim mergeability. | `WATCH`. Wait review; no third opencode PR without a distinct signal. |
| `cline#11087` | Open, `MERGEABLE`, merge-state blocked; SDK quality checks, Ubuntu/Windows tests, and Socket checks green after follow-up. | `PR-OPEN` / monitor only. Wait maintainer review. |
| `CopilotKit#5035` | Open, `MERGEABLE`, review required. Docs Vercel preview green; other Vercel contexts are team authorization gates. | `WATCH`. Wait maintainer review/team auth. |
| `pydantic-ai#5678/#5680` | Both open, `MERGEABLE`, merge-state clean, full CI/coverage/harness green. | `PR-OPEN`. Ready for maintainer review/merge; monitor only. |
| `E2B#1355` | Open maintainer/superset PR for JS + Python `fromDockerfile`/`from_dockerfile`; all visible SDK/CLI/CodeQL/Vercel/CLA checks green; review required, merge-state blocked. | `WATCH #1355`; no action on closed `#1354`. |
| `pydantic-ai#5688`, `google/adk-python#5864`, `langgraph#7688`, `continue#12334`, `goose#9136/#9332`, `MCP python-sdk#2702`, `cline#10931`, `OpenHands#14563` | No new maintainer/reporter signal that changes actionability. `google/adk-python#5864` is closest to actionable, but still assigned/request-clarification and needs reporter/maintainer direction. | `WATCH` / `COMMENT-FIRST` only; no upstream comment/PR now. |
| `trycua#1725/#1737`, MCP TS `#2115/#2116/#2138`, MCP Python `#2687/#2701`, `browser-use#4796/#4815`, `SWE-agent/mini-swe-agent#826` | Competing/open PRs remain active for most lanes. `mini-swe-agent#826` is still open with no comments, but broader search is rate-limited and adjacent timeout PR `#832` has merged, so retest is required before any claim. | `WATCH`. Do not duplicate; comment only with new verification or maintainer request. |

Repo-candidate queue: `microsoft/playwright-mcp` is proposed as the next bounded scouting target, not a PR/comment target in this cycle. Start with `#1635`, contribution/assignment gate, duplicate PR search, and repo-source location; keep `#1636` as secondary watch. Final `next_status`: `WATCH`.

3-subagent critique: factology rejected any `duplicate clean` wording for `mini-swe-agent#826` because GitHub Search API is rate-limited. Process gate passed for internal tracker/watch update only; upstream PR/comment fails without a new fact, maintainer request, repro, or CI diagnosis. Actionability selected exactly `WATCH`.

## Check 18:00 UTC

Bounded follow-up with 6 subagents over active PRs, stale/closed lanes, comment-first watch, duplicate-race lanes, repo-candidate queue, and tracker consistency. Parent live gates used `gh pr view` / `gh issue view`. No upstream PR/comment opened.

| Upstream | Current state | Action |
|---|---|---|
| `aaif-goose/goose#9447` | Open, `MERGEABLE`, review required, merge-state blocked. `changes` still failed from GitHub diff API transient; dependent jobs skipped; `check-quarantined` and `machete` green. | `WATCH`. Wait maintainer rerun/review; no duplicate CI comment. |
| `OpenHands/software-agent-sdk#3394` | Open; GitHub still reports old `CHANGES_REQUESTED`; mergeability unknown in latest view, check rollup empty. Maintainer eval/re-review remains the gate. | `WATCH`. Wait eval or maintainer review. |
| `opencode#29565/#29530` | Both open; duplicate/standards/compliance checks green; no maintainer review yet. | `WATCH`. Wait review; no third opencode PR without a distinct signal. |
| `cline#11087` | Open, `MERGEABLE`, CI green; formal approval still absent. | `WATCH`. Wait maintainer/Greptile re-review. |
| `CopilotKit#5035` | Open, review required. Docs preview green; other Vercel contexts are team authorization gates. | `WATCH`. Wait maintainer review/team auth. |
| `pydantic-ai#5678/#5680` | Both open, `MERGEABLE`, merge-state clean, full CI/coverage green. | `PR-OPEN`. Ready for maintainer review/merge; monitor only. |
| `E2B#1355` | Open maintainer/superset PR for JS + Python `fromDockerfile`/`from_dockerfile`; all visible SDK/CLI/CodeQL checks green; review required, merge-state blocked. | `WATCH #1355`; no action on closed `#1354`. |
| `deepagents#3587/#3616` | Issue `#3587` is open with `waiting-on-author`; maintainer needs raw Qwen/LiteLLM tool-call payload/provider details. PR `#3616` remains closed by assignment guard. | `WATCH / NO PR`. Do not PR empty-id acceptance without proof this is Deep Agents contract, not provider payload. |
| `pydantic-ai#5688`, `google/adk-python#5864`, `langgraph#7688`, `continue#12334`, `goose#9136/#9332` | No new maintainer/reporter signal that changes actionability. LangGraph repro comment remains minimized; Continue still needs reporter path; goose lanes remain assignment/design-sensitive. | `WATCH` / `COMMENT-FIRST` only; no upstream comment/PR now. |
| `trycua#1725/#1737`, MCP TS `#2115/#2116/#2138`, MCP Python `#2687/#2701`, `browser-use#4796/#4815` | Competing/open PRs still active; issues remain open. MCP TS has two green competing PRs; MCP Python `#2701` has one Windows locked failure but aggregate `all-green` success. | `WATCH`. Do not duplicate; comment only with new verification or maintainer request. |

Repo-candidate queue update: follow-up cycle 27 completed the `SWE-agent/mini-swe-agent` scan and kept final status `WATCH`, not PR-ready. Internal lane [#53](https://github.com/serejaris/corp-opensource/issues/53) tracks `mini-swe-agent#826`: temp-clone repro saw orphan child process after timeout, but GitHub Search API was rate-limited and dedicated runner repro is still missing. Future cycle must run `corp-opensource-runner` repro, repeat PR/search gates, and fill a regression card before any upstream action.

Tracker consistency actions for this pass: update README dashboard timestamp/status, add explicit `openai/openai-agents-python` WATCH row, mark old E2B rows as historical/superseded, and update repo-card wording after cycle 25. Final `next_status`: `WATCH`.

## Check 17:15 UTC

Bounded cycle with 6 subagents, parent live gates, and 3-subagent critique. No upstream PR/comment opened.

| Upstream | Current state | Action |
|---|---|---|
| `trycua/cua#1725` / `#1737` | Issue `#1725` is still open, label `bug`, no assignee. New PR `#1737` is open, non-draft, `MERGEABLE`, review required, merge-state blocked. Its body says `Closes #1725` and it touches the recorder core, recording loader, driver registry wiring, and Windows recording hooks. CodeRabbit is green; Vercel failure is team authorization. Windows smoke is not independently proven. | `next_status: WATCH`. Watch `#1737` review/merge or Windows smoke. Do not open competing PR/comment unless maintainers ask, `#1737` closes without merge, or smoke proves the contract still fails. |
| `aaif-goose/goose#9447` | Open, mergeable, review required. `changes` still failed from GitHub diff API transient; dependent jobs skipped; `check-quarantined` and `machete` green. | Wait maintainer rerun/review; no code action. |
| `anomalyco/opencode#29530` | Open, mergeable; duplicate/standards/compliance checks green. | Watch maintainer review. |
| `CopilotKit/CopilotKit#5035` | Open, mergeable, review required. Docs preview green; other Vercel contexts need team authorization. | Watch maintainer review/team auth; not a code failure. |
| `OpenHands/software-agent-sdk#3394` | Open, mergeable, old `CHANGES_REQUESTED` still visible after follow-up; check rollup empty and eval not visible as a completed gate. | Wait maintainer eval/workflow approval. |
| `pydantic-ai#5688`, `google/adk-python#5864`, `continue#12334`, `MCP TypeScript#2115` | No new maintainer/reporter signal that changes actionability. MCP TypeScript remains duplicate-race with `#2116/#2138`; Continue remains needs-reporter-path. | Watch only. |

Critique result: fact-check rejected `duplicate-covered` wording for `trycua#1725`; correct wording is competing PR pending review. Process gate passed because the tracker update follows 6-subagent scouting and parent live gates. Actionability chose exactly `WATCH` with unblock event `#1737` merge/close, maintainer request, or Windows smoke result.

## Check 09:05 -03

Live follow-up after cycle 18 scouting and E2B duplicate cleanup.

| Upstream | Current state | Action |
|---|---|---|
| `e2b-dev/E2B#1354` | Closed by us as superseded. It only covered JS `Template.fromDockerfile` and was CLA-blocked. | Done. Track `#1355` instead. |
| `e2b-dev/E2B#1355` | Open maintainer/superset PR. Covers JS `Template.fromDockerfile` and Python `Template.from_dockerfile`, includes multi-source `COPY`/`ADD` regressions. Historical note: later 18:00 UTC follow-up saw all visible SDK/CLI checks green while PR remained review-required. | Watch `#1355`; no duplicate PR. |
| `modelcontextprotocol/python-sdk#2701` | Open, mergeable. Covers `#2687` with redirect URI normalization and shared auth regression; most CI green, one Windows locked job failed while all-green reported success. | Watch existing PR; no new PR for `#2687`. |
| `pydantic-ai#5678/#5680/#5681` | Open, mergeable, CI green in tracked state. | Watch. |
| `pydantic-ai#5688` | Still open, no assignee, pydanty root-cause comment + our offer; no exact PR found in tracked state. | Wait assignment/confirmation; local patch prep exists. |
| `google/adk-python#5864` | Still open, assigned, `request clarification`; no reply after our offer. | Wait maintainer/reporter direction; local patch prep exists. |
| `langgraph#7688` | Still open; our TIME_WAIT repro comment now has one thumbs-up; no maintainer direction yet. | Watch for package-surface direction. |
| `OpenHands/software-agent-sdk#3394` | Open, mergeable, maintainer wants eval; reviewDecision still `CHANGES_REQUESTED` due earlier bot review despite follow-up. | Wait eval/human maintainer. |
| `opencode#29530/#29565`, `cline#11087`, `CopilotKit#5035` | Still open; no new code-action comments in tracked state. | Watch maintainer review. |

## Check 09:20 -03

Live follow-up after opening `goose#9447`.

| Upstream | Current state | Action |
|---|---|---|
| `aaif-goose/goose#9447` | Open, ready, mergeable, review required. `check-quarantined` and `machete` passed. `changes` failed because GitHub API returned `diff temporarily unavailable due to heavy server load`; dependent Rust/release/live-provider jobs skipped. Rerun attempt via `gh run rerun --failed` was not allowed. Local `cargo clippy -p goose --lib -- -D warnings` passed; PR comment posted with CI note. | Watch CI/review; ask maintainer rerun if needed. |
| `aaif-goose/goose#9446` | PR `#9447` linked as fix for the missing `response.output` parser failure. | Track through PR. |

Decision: no fresh upstream PR this pass. The productive move was closing the now-duplicate E2B PR and recording new candidates for the next regression-first pass.

## Check 10:35 -03

Live follow-up after opening `opencode#29565`.

| Upstream | Current state | Action |
|---|---|---|
| `anomalyco/opencode#29565` | Open, ready, mergeable. `check-standards`, `check-compliance`, `check-duplicates`, `add-contributor-label` all green. No comments/reviews yet. | Watch maintainer review; do not open another opencode PR unless a distinct gap appears. |
| `anomalyco/opencode#29530` | Open, mergeable. Duplicate/compliance/standards checks green; GitHub bot says contribution guidelines pass. | Watch maintainer review. |
| `OpenHands/software-agent-sdk#3394` | Open. Maintainer said it looks fine but wants eval. Latest fork workflows on `3c8eae78` are still `action_required`, no jobs. | Wait maintainer eval/workflow approval. No code action. |
| `e2b-dev/E2B#1354` | Historical snapshot before cleanup: PR was still open and CLA-blocked. Superseded later by maintainer/superset `#1355`; `#1354` is now closed. | Historical only; track `#1355`, no action on `#1354`. |
| `cline/cline#11087` | Open, mergeable. Greptile gap addressed; visible SDK/quality/security checks green or skipped intentionally. | Watch maintainer review. |
| `CopilotKit/CopilotKit#5035` | Open, mergeable, review required. Docs Vercel preview green; other Vercel previews need team authorization; Claude automated review disabled for fork. | Watch maintainer review / team preview authorization. |
| `pydantic/pydantic-ai#5678` | Open, mergeable. CI matrix, coverage, smokeshow green. Codex review quota warning only. | Watch maintainer review. |
| `pydantic/pydantic-ai#5680` | Open, mergeable. CI matrix, coverage, smokeshow green. Codex review quota warning only. | Watch maintainer review. |
| `pydantic/pydantic-ai#5681` | Open, mergeable. CI matrix, coverage, smokeshow green. | Watch/review only; duplicate-covered lane. |
| `langchain-ai/deepagents#3616` | Closed by assignment guard. Checks cancelled/failing because PR was auto-closed, not code signal. Later follow-up moved the gate to issue `#3587` waiting on raw Qwen/LiteLLM payload details. | No second PR; wait author/provider evidence. |

## Decision

No active upstream PR needs a code fix right now. Continue strategy by running the next scouting cycle while monitoring existing PRs.

## Check 08:10 -03

Live follow-up after cycle 15 scouting.

| Upstream | Current state | Action |
|---|---|---|
| `pydantic-ai#5688` | Open; pydanty confirmed root cause and says `PR-ready -> delegating to PR opener`; no exact open PR found. | Comment-offer posted; wait assignment/confirmation or pydanty PR before opening anything. |
| `google/adk-python#5864` | Open; our comment-offer is the latest external action; still `request clarification`. | Wait maintainer/reporter direction. |
| `OpenHands/software-agent-sdk#3394` | Open/mergeable; no status checks reported; maintainer wants eval; old bot `CHANGES_REQUESTED` review remains in GitHub reviewDecision. | Wait eval / maintainer action. |
| `E2B#1354` | Historical snapshot before cleanup: CLA-blocked at the time; superseded later by maintainer/superset `#1355` and closed. | Historical only; no action on `#1354`. |

Decision: no new PR now. Best next technical branch is `pydantic-ai#5688`, but only after assignment/confirmation because bot delegation is already in flight.

## Check 08:25 -03

Live duplicate gate on the fallback candidates from cycle 15.

| Candidate | Current state | Action |
|---|---|---|
| `cline#11065` | Covered by `cline#11084` from the issue author. | No PR. |
| `modelcontextprotocol/python-sdk#2689` | Covered by `python-sdk#2698`. | No PR. |
| `opencode#29544` | Covered by `opencode#29541`. | No PR. |
| `CopilotKit#4935` | Covered by `CopilotKit#4947` and overlapping `#4948`. | No PR. |
| `pydantic-ai#5688` | Still open, no assignment, no exact PR found; our availability comment remains latest. | Watch; do not race pydanty delegation. |
| `google/adk-python#5864` | Still `request clarification`; no reply to our PR offer. | Watch. |

Decision: keep the queue in watch mode. The next implementation opportunity is still `pydantic-ai#5688`, but only after repo gate opens or after comparing the delegated PR if it appears.
