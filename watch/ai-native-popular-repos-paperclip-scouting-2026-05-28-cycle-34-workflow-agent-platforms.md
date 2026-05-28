# AI-native workflow / agent platform scouting, 2026-05-28 cycle 34

Trackers: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52).

Scope:

- `HKUDS/nanobot`
- `coze-dev/coze-studio`
- `triggerdotdev/trigger.dev`
- `lastmile-ai/mcp-agent`
- `julep-ai/julep`
- `PySpur-Dev/pyspur`

The required `open-source-bug-scouting` skill is not installed in this environment, so this cycle used documented fallback: live GitHub repo/issue/PR gates, 6 read-only scouting roles in two waves, then 3-role critique before tracker updates.

Upstream action count: `0`.

No upstream comment, PR, ping, rerun, or rebase was made.

## Parent Live Gates

Live time: `2026-05-28 02:36-02:45 UTC`.

Runner/tooling gate:

- `corp-opensource-runner` is still unavailable from this environment.
- Local environment has Node `v20.19.2` and Python `3.13`, but no `pnpm`, `uv`, `bun`, Docker, Rust toolchain, or stable service-stack runner.
- This was a repo-universe scouting cycle, so no heavy repro was attempted locally. Any promotion beyond `LEAD` needs runner-backed evidence or a narrow local static/source gate.

| Repo / lane | Live result | Decision |
|---|---|---|
| `coze-dev/coze-studio#2683` | Open, unassigned, unlabeled, no comments. Detailed Signal Arena / Agent World auth failure report; no visible exact PR cover in quick PR search. Parent code search for `world.coze.site` in `coze-dev/coze-studio` returned `0`, so OSS ownership is unproven and the bug may be hosted/config infra. | `LEAD`, not `CANDIDATE`. |
| `HKUDS/nanobot#4006` | Open bug, but exact open PR `#4011` says `Fixes #4006`, touches `nanobot/agent/loop.py` and tests, and is waiting review. | `WATCH / duplicate-covered`. |
| `HKUDS/nanobot#4013` | Open bug with maintainer root-cause/workaround. Exact open PR `#4020` says `Closes #4013` and implements per-provider stream-idle timeout config. | `WATCH / duplicate-covered`. |
| `lastmile-ai/mcp-agent#671` | Open issue, but exact open PRs `#675` and `#686` both cover failed capability lookup filtering in `src/mcp_agent/agents/agent.py` with tests. | `WATCH / duplicate-covered`. |
| `PySpur-Dev/pyspur#294` | Open issue, but exact open PR `#302` says `Fixes #294` and pins `click<8.2`. | `WATCH / duplicate-covered`; re-enter only if `#302` closes/stalls and current package still fails. |
| `triggerdotdev/trigger.dev#3672` | Strong waitpoint metrics attribution bug. Exact PR `#3724` was closed unmerged. Repo requires contributor vouch and unvouched PRs are auto-closed. | `COMMENT-FIRST / WATCH`; no blind PR. |
| `triggerdotdev/trigger.dev#3674` | Strong self-hosted ClickHouse URL regression. Exact PR `#3681` was closed unmerged. Repo requires contributor vouch. | `COMMENT-FIRST / WATCH`; no blind PR. |
| `triggerdotdev/trigger.dev#3726` | Plausible `ExponentialBackoff.execute` max-elapsed bug; no exact open PR found, but prior attempts/nearby retry PRs and vouch gate make it process-blocked. | `WATCH`; needs maintainer/vouch signal. |
| `triggerdotdev/trigger.dev#3741` | Deploy failure around `registry.depot.dev 401`; no exact PR found, but likely infra/secrets-owned and not locally reproducible. | `WATCH / likely infra-owned`. |
| `julep-ai/julep#1595/#1602` | Security-sensitive public issues in a cold repo with hosted shutdown/lifecycle concerns. No exact cover found, but process risk is high. | `NO-GO bug-hunting / archival WATCH`. |
| `PySpur-Dev/pyspur#288` | Stale `raw_response` issue with no exact cover found; no fresh comments or current-main repro. | Low-priority `WATCH`. |

## 6-Role Synthesis

- Repo-fit: `HKUDS/nanobot` is the strongest active Python agent runtime in this slice, but its best current bugs are already covered by live PRs. `trigger.dev` is active and strategically relevant but has a hard vouch gate. `mcp-agent` has excellent MCP fit but weak merge cadence. `julep` is watch-only because of lifecycle/shutdown signals. `PySpur` has strong visual workflow fit but weak maintainer cadence.
- Bug-signal: the best fresh unowned signal is `coze-dev/coze-studio#2683`; it has detailed user-facing auth evidence and no visible maintainer response. However, the failing path may be hosted infrastructure, not OSS code.
- Repro path: `PySpur#294`, `mcp-agent#671`, and `trigger.dev#3726` are technically easy repro lanes, but `PySpur#294` and `mcp-agent#671` are exact-covered and `trigger.dev#3726` is process-gated.
- Patchability: no lane is suitable for a fresh PR now. Patchable lanes are either duplicate-covered, vouch-gated, security/process-sensitive, or OSS-ownership-unproven.
- Duplicate race: exact covers exist for `nanobot#4006/#4013`, `mcp-agent#671`, `PySpur#294`, and `Julep#1550`. Trigger exact attempts for `#3672/#3674` were closed unmerged by process, not by technical resolution.
- PR-readiness: no `PR-READY` lane. The most useful forward movement is a LEAD-level Coze ownership/source gate and watch re-entry triggers for duplicate-covered lanes.

## 3-Role Critique

- Factology: downgrade the proposed `coze#2683 CANDIDATE` to `LEAD`. Parent code search for `world.coze.site` returned `0`, and code ownership is not proven. Say only that no visible exact PR duplicate was found by keyword/PR search, not that no duplicate exists globally.
- Process: tracker/watch update is allowed because the cycle completed 6 roles, parent live gates and 3-role critique. No new candidate issue is appropriate until `coze#2683` is proven repo-owned.
- Actionability: no upstream comment/PR. Use one umbrella watch update and unblock events instead of per-repo noise.

## Decision

`next_status: LEAD`

Selected lead: `coze-dev/coze-studio#2683`.

This is not `CANDIDATE` because the failing auth URL/config was not found in OSS code during parent source search.

Unblock events:

- `coze-studio#2683`: broader source search finds Signal Arena / Agent World verify-key URL or stale-domain mapping in OSS, or a maintainer confirms the failing auth path is repo-owned.
- `coze-studio#2683`: a reproducible self-host/local path appears that does not require hosted credentials or private infra.
- `trigger.dev#3672/#3674/#3726`: maintainer grants vouch path, reopens or requests follow-up on closed exact attempts, or explicitly asks for a new PR.
- `nanobot#4011/#4020`, `mcp-agent#675/#686`, `PySpur#302`: exact-cover PR closes or merges without resolving the issue.
- `Julep#1595/#1602`: official security triage or maintainer acceptance signal appears and repo cadence revives.

