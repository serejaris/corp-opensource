# Infisical/agent-vault scouting, 2026-05-27

Live state: parent `gh` pass на 2026-05-27 19:45-19:53 UTC.

Upstream: [`Infisical/agent-vault`](https://github.com/Infisical/agent-vault).
Internal tracker: [`corp-opensource#60`](https://github.com/serejaris/corp-opensource/issues/60).
Umbrella tracker: [`corp-opensource#52`](https://github.com/serejaris/corp-opensource/issues/52).

Upstream actions в этом блоке: 0 PR, 0 comments.

Final `next_status`: `WATCH`.

Required skills `open-source-bug-scouting` и `open-source-pr-workflow` в текущей session недоступны, поэтому использован documented fallback: 6 read-only scouting subagents, parent live gates, затем 3-subagent critique.

## Parent Live Gates

| Gate | Result |
|---|---|
| Repo state | `Infisical/agent-vault`: 1443 stars, 73 forks, default branch `main`, not archived, not fork, latest release [`v0.22.0`](https://github.com/Infisical/agent-vault/releases/tag/v0.22.0) published `2026-05-26T03:40:24Z`, pushed `2026-05-26T03:30:42Z`, updated `2026-05-27T15:26:01Z`. |
| Queue pressure | 4 open issues, 10 open PRs. Recent merged PRs include [`#206`](https://github.com/Infisical/agent-vault/pull/206) external credential stores and [`#195`](https://github.com/Infisical/agent-vault/pull/195) MITM Host header fix. |
| Product fit | Strong infra-adjacent Paperclip-like fit: HTTP credential proxy/vault for AI agents, secure agent sandbox credential boundary, Claude Code/OpenClaw/Hermes/Codex/OpenCode adjacency. |
| Best lead | [`#194`](https://github.com/Infisical/agent-vault/issues/194): Codex CLI Responses WebSocket transport fails with Agent Vault HTTPS proxy URL. Report has container setup, versions, stderr, and comparison checks. |
| Duplicate / PR search | No exact open PR found for `#194`. Historical WebSocket support [`#123`](https://github.com/Infisical/agent-vault/pull/123) is already merged and must not be duplicated. Active MITM/proxy overlap exists in [`#196`](https://github.com/Infisical/agent-vault/pull/196), [`#197`](https://github.com/Infisical/agent-vault/pull/197), [`#198`](https://github.com/Infisical/agent-vault/pull/198), and [`#207`](https://github.com/Infisical/agent-vault/pull/207). |
| Process gate | No root `CONTRIBUTING.md`, no issue templates, PR template exists with summary/type/test plan/security checklist. Security policy says undisclosed vulnerabilities must go to `security@infisical.com`, not public issues. |
| License gate | GitHub reports `licenseInfo: Other`, community profile `spdx_id: NOASSERTION`. `LICENSE` text grants MIT Expat rights for content outside possible `ee/` directories; current root listing did not show `ee/`. Do not touch license/enterprise areas without maintainer-aligned gate. |
| Runner gate | No runner repro in this block. `#194` was reported against Agent Vault `0.21.0` and `0.15.0`; current latest is `v0.22.0`, so current-main/latest repro is still missing. |
| Secret gate | Any future repro must use synthetic credentials only. Do not log or commit `AGENT_VAULT_MASTER_PASSWORD`, `AGENT_VAULT_TOKEN`, OpenAI/GitHub/Anthropic keys, Infisical machine credentials, `.env`, or `~/.agent-vault/` data. |

## 6-Subagent Synthesis

| Role | Finding |
|---|---|
| repo-fit | Active young repo with strong AI-agent security/control-plane relevance. Keep active watch/repo-card; API churn and open PR backlog raise stale/duplicate risk. |
| issue triage | `#194` is the only credible bug lead. `#192` is feature-only, `#149` overlaps OAuth PR `#170`, and `#108` overlaps supply-chain PRs `#138/#139`. |
| PR / duplicate pressure | No exact `#194` PR found, but any generic "add WebSocket support" PR would duplicate merged `#123`. MITM/proxy files are crowded by `#196/#197/#198/#207`. |
| process / license / security | Public compatibility comment is allowed only if secret-free and not framed as a vulnerability. PR needs test plan plus security checklist. License is `Other/NOASSERTION`, but non-`ee/` code appears MIT Expat by local text. |
| repro / runner | Secret-free baseline can use Go unit tests, web/SDK builds, fake upstream HTTP/HTTPS/WebSocket, and synthetic dummy credentials. Docker/network/isolation tests belong in `corp-opensource-runner`. |
| PR-readiness | `#194` is not `PR-READY`: Agent Vault-side patch surface is unproven because the report says normal Node `ws` through Agent Vault works and Codex rejects proxy URL before Agent Vault sees traffic. |

## 3-Subagent Critique

| Role | Result |
|---|---|
| factology / duplicates | Do not call `#194` a proven Agent Vault MITM bug. Phrase as Codex/Agent Vault integration or proxy compatibility issue. No exact duplicate found; `#123` is historical overlap only. |
| process gates | `#194` is open, unassigned, unlabeled, no linked PR. `v0.22.0` was released after the reporter's tested versions, so latest/current-main repro is required before code. |
| actionability | Split recommendation: two critiques allowed `COMMENT-FIRST`, one recommended stricter `WATCH`. Parent decision is conservative `WATCH`: do not upstream-comment until we can add a verified latest repro or a sharply scoped maintainer question. |

## Decision

Keep `Infisical/agent-vault` as active infra-adjacent watch, not a current upstream target.

Final `next_status`: `WATCH`.

Promotion gates:

1. Run secret-free current-main or `v0.22.0` repro for `#194` in dedicated runner.
2. Recheck exact duplicates and overlapping PRs, especially `#196/#197/#198/#207`.
3. Decide owner boundary: Agent Vault code/docs/workaround vs Codex CLI upstream.
4. If evidence adds a new fact, move to `COMMENT-FIRST` with a narrow maintainer question. Do not open PR until maintainer direction or fail-before/pass-after evidence identifies Agent Vault-owned patch surface.
