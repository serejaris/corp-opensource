# Popular AI-native repo universe cycle 32, 2026-05-28

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52), selected candidate [#84](https://github.com/serejaris/corp-opensource/issues/84).
Tracker comments: [#52 comment](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560104875), [#84 comment](https://github.com/serejaris/corp-opensource/issues/84#issuecomment-4560107476).

Bounded repo-universe refresh over browser/control-plane/code-context/sandbox repos:

- `vercel-labs/agent-browser`
- `oraios/serena`
- `multica-ai/multica`
- `colbymchenry/codegraph`
- `epiral/bb-browser`
- `agent-infra/sandbox`

The required `open-source-bug-scouting` skill is not installed in this environment, so this cycle used documented fallback: parent live GitHub gates, 6 read-only scouting roles in two waves, then 3-role critique before tracker/watch updates.

Upstream action count: `0`.

No upstream comment, PR, ping, rerun, or rebase was made.

## Parent Live Gates

Live time: `2026-05-28 01:25 UTC`.

Runner/tooling gate:

- `corp-server` does not resolve from this environment.
- `corp-opensource-runner` is not provisioned per [#10](https://github.com/serejaris/corp-opensource/issues/10).
- Local toolchain: Node `v20.19.2`, Python `3.13.5`, no `rustc/cargo`, no `pnpm`, no `bun`, no Docker.
- Therefore no repo can move to `PR-READY` in this cycle.

Search caveat:

- GitHub Search API hit a rate limit during targeted PR search for `serena#1519` and `codegraph#507`.
- Parent compensated with `gh issue list`, `gh pr list`, and targeted `gh issue/pr view`, but duplicate search for any future upstream action must be repeated after the rate limit clears.

| Repo | Live repo state | Best live lane | Decision |
|---|---|---|---|
| `vercel-labs/agent-browser` | `34,481` stars, Apache-2.0, pushed `2026-05-28`, latest `v0.27.0`, `255` issues / `236` PRs. | Fresh issues are mostly under-documented or exact-covered: `#1391 -> #1392`, `#1380 -> #1382/#1388`, `#1379 -> #1381`, `#1353 -> #1354`, `#1349 -> #1364`. | `WATCH`; hot repo, but duplicate-race too high for candidate promotion. |
| `oraios/serena` | `24,691` stars, MIT, pushed `2026-05-27`, latest `v1.5.3`, `73` issues / `32` PRs. | `#1519` dashboard 403 when Docker host port differs from internal dashboard port. | `CANDIDATE` via [#84](https://github.com/serejaris/corp-opensource/issues/84); source-confirmed but runner and fresh duplicate search required. |
| `multica-ai/multica` | `33,664` stars, custom/NOASSERTION license, pushed `2026-05-27`, latest `v0.3.10`, `394` issues / `402` PRs. | `#3403` agent status stuck after runtime timeout is under-specified; many runtime/daemon issues have nearby PRs. | `WATCH`; high PR queue and license/process risk block code action. |
| `colbymchenry/codegraph` | `29,898` stars, MIT, pushed `2026-05-27`, latest `v0.9.6`, `85` issues / `102` PRs. | `#507` Hermes YAML indentation looked strong, but open PR `#345` exactly covers the list-indentation fix and current `main` appears already patched. | `WATCH / duplicate-covered`; no independent action while `#345` is live. |
| `epiral/bb-browser` | `5,540` stars, MIT, pushed `2026-05-27`, latest `bb-browser-v0.11.6`, `61` issues / `29` PRs. | `#217` Windows daemon default port `19824` conflicts with IP Helper; no exact PR found in current evidence. | `WATCH`; future mode may be comment-first after Windows/port-policy validation and runner evidence. |
| `agent-infra/sandbox` | `4,820` stars, Apache-2.0, pushed `2026-05-22`, latest `v1.0.0.152`, `69` issues / `19` PRs. | `#193` `no_change_timeout` ignored, but comments indicate image/SDK mismatch and server/source boundary is unclear. | `LEAD`; needs source-boundary evidence and container repro before candidate promotion. |

## 6-Role Synthesis

- Repo fit/activity: `serena`, `agent-browser`, `multica`, `codegraph`, `bb-browser`, and `sandbox` all match Paperclip-adjacent / AI-native strategy, but only `serena` has a clean enough issue/process mix today.
- Bug signal: strongest fresh issue is `serena#1519`; `codegraph#507` is strong but covered by PR `#345`; `bb-browser#217` is plausible but Windows/policy-heavy.
- Duplicate/race: `agent-browser`, `multica`, and `codegraph` have severe PR queue pressure. Many fresh bugs are already exact-covered by adjacent-number PRs.
- Process: `serena` has contribution/test docs and a plausible focused test path; `multica` has custom license risk; `agent-browser` lacks obvious contribution docs; `bb-browser` lacks contribution docs but has issue templates; `sandbox` has contribution docs but runtime/image source boundary is unclear.
- Source plausibility: `serena#1519` maps to `src/serena/dashboard.py`, `SerenaDashboardAPI.run()`, with a host-header check that uses the internal Flask port.
- Runner priority: first future repro target is `serena#1519`; fallback is `bb-browser#217` only after Windows/port policy clarity. `codegraph#507` is not a runner target while PR `#345` is open.

## 3-Role Critique

- Factology/duplicates: `codegraph#507` must be demoted because PR `#345` covers the same YAML indentation fix and current `main` appears patched. `serena#1519` remains valid, but duplicate search completeness is limited by the Search API rate limit.
- Process gates: internal tracker/watch updates are allowed; upstream `COMMENT-FIRST`, `PR-READY`, and PR are not allowed without runner-backed evidence, complete duplicate search, and a fresh 3-role critique.
- Actionability: select `oraios/serena#1519` as the only internal `CANDIDATE`. Keep `bb-browser#217` as watch/future comment-first and keep `agent-browser`, `multica`, `codegraph`, and `sandbox` out of upstream action.

## Decision

`next_status: CANDIDATE`

Selected lane: `oraios/serena#1519`.

Upstream action count: `0`.

Runner action count: `0`.

## Regression Card Draft: `serena#1519`

Contract: dashboard host validation should not reject a legitimate Docker host-port mapping when the dashboard is intentionally bound for external access.

Likely patch/test surface:

- `src/serena/dashboard.py`
- `test/serena/test_dashboard.py`

Fail-before idea:

1. Construct or run the dashboard with internal port `24282`.
2. Send a request to `/dashboard/` or `/dashboard/index.html` with `Host: localhost:34283`.
3. Current behavior: `403 Forbidden`.
4. Control request with `Host: localhost:24282` should not be rejected by the host-header gate.

Re-entry gate:

- Rerun PR search for `1519`, `dashboard`, `403`, `Docker host port`, `host header`, and `allowed hosts`.
- Run a focused unit or Docker repro in `corp-opensource-runner`.
- Decide with 3-role critique whether the correct upstream mode is `COMMENT-FIRST` for host-validation design or `PR-READY` with a minimal test-backed patch.

## Follow-up Gate Refresh, 2026-05-28 01:46 UTC

Bounded follow-up for `oraios/serena#1519` after GitHub Search API rate limit cleared.

Tracker comment: [#84 follow-up comment](https://github.com/serejaris/corp-opensource/issues/84#issuecomment-4560148164).

Fallback/process note: required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed here, so this follow-up used parent live gates, 6 read-only roles in two waves, then 3-role critique. Upstream action count remains `0`; no upstream comment, PR, ping, rerun, or rebase was made.

Parent live gates:

- `oraios/serena#1519` is still open, with no assignees, labels, or comments.
- Repo state: `24,692` stars, MIT, default branch `main`, latest release `v1.5.3`, pushed `2026-05-27T11:31:58Z`, `73` issues / `32` PRs.
- Current `main` SHA: `7bf30080f6aa8fc1f983e1d72bc91dd790f28d29`.
- Open PR list still has no dashboard host-port/allowed-hosts PR. Targeted open PR searches for `1519`, `dashboard 403`, `host header`, and `dashboard allowed hosts OR web_dashboard_allowed_hosts OR trusted hosts` returned `[]`.
- Broader issue/PR searches found no covering duplicate. Nearby context is not a duplicate: old PR `#403` is unrelated MCP stdio/dashboard auto-open work; issue `#1208` is dashboard security context; PRs `#719` and `#1509` are dashboard-adjacent but do not cover host-port validation; PRs `#1227/#1223` are download-host allowlisting, not dashboard trusted hosts.
- Runner remains unavailable: `corp-server` does not resolve, and no dedicated `corp-opensource-runner` is provisioned; no Docker or heavy local repro was run.

6-role follow-up synthesis:

- Factology/duplicates: duplicate risk is low after the cleared-rate-limit searches; no covering PR/issue was found.
- Process: Serena allows direct small bugfix PRs, but `COMMENT-FIRST`/`PR-READY` still need current evidence. CI/test commands are `uv sync --extra dev --locked`, `uv run poe lint`, `uv run poe test -q --tb=short`, and `uv run poe type-check`.
- Source/test plausibility: `src/serena/dashboard.py:770-786` registers the `before_request` host check with `allowed = {f"127.0.0.1:{port}", f"localhost:{port}"}`. With Docker mapping `34283:24282`, `Host: localhost:34283` can be rejected because `port` is the internal Flask port. Existing light test surface is `test/serena/test_dashboard.py`.
- Runner/regression card: fail-before should show host-side `curl http://localhost:34283/dashboard/index.html` returning `403` while in-container `localhost:24282` returns `200`; post-fix should make the mapped loopback case non-403 while rejecting an unrelated hostile `Host`.
- Security/design guardrail: do not fix by wildcarding hosts, skipping validation when bind address is `0.0.0.0`, trusting `SERENA_DOCKER=1`, or using `ProxyFix` to bypass Host validation. Safe directions are explicit configured `host[:port]` allowlist entries or a tightly scoped loopback-host rule with negative tests.

3-role critique:

- Factology/duplicates: keep `CANDIDATE`; no factual reason to demote and no covering duplicate was found.
- Process gates: keep `CANDIDATE`; upstream `COMMENT-FIRST`/`PR-READY` is premature without runner-backed repro or documented minimal repro evidence.
- Actionability/security dissent: `COMMENT-FIRST` could be useful to steer away from wildcard/skip-host-check fixes, but it is still an upstream action without local evidence. Parent decision: record the security guardrail internally and wait for runner-backed evidence before any upstream comment.

Decision: `next_status: CANDIDATE`.

Closed caveat: duplicate search is no longer rate-limited for `#1519`.

Remaining blockers before upstream action:

- Runner-backed Docker smoke or accepted documented fallback with exact current-main fail-before logs.
- Focused unit regression around the host-validation helper/hook.
- Repeat live gates immediately before any upstream comment/PR.
- 3-role critique after repro to choose exactly one of `COMMENT-FIRST` or `PR-READY`.
