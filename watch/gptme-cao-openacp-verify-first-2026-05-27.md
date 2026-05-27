# gptme / CAO / OpenACP verify-first scouting, 2026-05-27

Tracker: [#69](https://github.com/serejaris/corp-opensource/issues/69)
Umbrella: [#52](https://github.com/serejaris/corp-opensource/issues/52)
Decision time: 2026-05-27 21:27 UTC
Final `next_status`: `WATCH`

## Scope

Bounded verify-first repo slice for lower-noise terminal-agent and control-plane repos.

| Repo | Fit | Live gate |
|---|---|---|
| `gptme/gptme` | Terminal-first local agent with local tools, shell, file editing, browser/web, ACP and Tauri-adjacent surfaces. | MIT; default `master`; created `2023-03-24`; `4311` stars / `390` forks; `10` open issues / `1` open PR; latest release `v0.31.0` published `2025-12-15T10:16:33Z`; pushed `2026-05-27T16:32:55Z`. |
| `awslabs/cli-agent-orchestrator` | Multi-agent CLI orchestrator over tmux/provider CLIs; close to Paperclip control-plane reliability, inbox delivery, provider state and worktree orchestration. | Apache-2.0; default `main`; created `2025-07-29`; `638` stars / `115` forks; `23` open issues / `10` open PRs; latest release `v2.1.1` published `2026-04-28T04:38:03Z`; pushed `2026-05-26T13:55:46Z`. |
| `Open-ACP/OpenACP` | ACP messaging bridge for coding agents and messaging platforms; useful protocol/ecosystem watch. | MIT; default `main`; created `2026-03-19`; `401` stars / `46` forks; `4` open issues / `1` open PR; latest release `v2026.518.2` published `2026-05-18T04:17:29Z`; pushed `2026-05-18T04:17:21Z`. |

Required skills note: local skills `open-source-bug-scouting` / `open-source-pr-workflow` are not installed as callable skills in this session, so this cycle used the documented fallback: parent GitHub live gates plus 6-subagent scouting. No upstream comment/PR was attempted.

## Parent live gates

### gptme/gptme

- Community profile: health `62`, MIT license, README and contributing docs; no root PR template found.
- Contribution gate: `docs/contributing.rst` is WIP, but asks for setup via `poetry`/`pipx`, `make build`, `make test`; slow LLM tests are optional.
- Test/repro gate: Python `>=3.10`, Poetry stack, optional browser/computer/server extras; GUI/Tauri lanes need desktop-capable runner.
- Issue shape:
  - `#554` Better subagents: active and relevant, but design-heavy.
  - `#1591` Tauri E2E tests: open, but comments mention draft PR `#2456`; needs gap check rather than fresh PR.
  - `#523`, `#607`, `#216`: high-value reliability/tool/computer-use lanes, but marked needs-design/hard.
  - `#2193` desktop onboarding/settings: code-side mostly already closed by prior PR comments; remaining work is manual validation/gap list.
- PR shape: only visible open PR is old draft `#494` around OpenRouter stream interruption.

### awslabs/cli-agent-orchestrator

- Community profile: health `75`, Apache-2.0, README, CONTRIBUTING, PR template inherited from awslabs.
- Contribution gate: check existing/recent issues and PRs, open an issue for significant work, keep PR focused, run local tests.
- Test/repro gate: Python `>=3.10`, `uv`, `tmux >=3.2`; unit tests are secret-free, integration/e2e requires provider CLIs/auth/tmux.
- Issue shape:
  - `#78` prompt not always returned after inbox messages: strong bug signal, intermittent terminal/raw-mode behavior, needs fresh repro.
  - `#131` pending messages stuck / brittle status regex: best later watch lane, but linked to `#115` and needs current-main repro.
  - `#164` duplicate inbox race: relevant but overlaps event-driven/message refactor.
  - `#151`, `#153`: covered by PRs `#261/#255` and `#260`.
  - `#100`: worktree workflows, design-heavy and assigned to many people.
- PR shape: active overlap with `#115` Event driven architecture and `#251` eager inbox delivery; also `#235` Codex handoff/state isolation, `#207` Windows support, memory PRs `#254/#179`.

### Open-ACP/OpenACP

- Community profile: health `75`, MIT, README, CONTRIBUTING, PR template.
- Contribution gate: Node `>=20`, pnpm `10.5+`, strict TypeScript/ESM/Zod/Pino, docs update for features, maintainer review, `pnpm build` and `pnpm test`.
- Issue shape:
  - `#247` session-list status filter: already covered by open PR `#251`.
  - `#240` Discord rate limiting: comment says fixed in adapter commit.
  - `#232` duplicate Discord channels: comment says fixed in adapter commits/migration.
  - `#243` Discord mention feature: product/design feature, not a bugfix lane.
- PR shape: open PR `#251` covers `#247`.

## 6-subagent synthesis

| Role | Result |
|---|---|
| Repo fit/activity | Rank: `gptme` for tactical terminal-agent fit, `awslabs/cli-agent-orchestrator` for strategic multi-agent control-plane fit, `OpenACP` as protocol reference. Recommended `WATCH`. |
| Issue quality/actionability | No clean `CANDIDATE`. Best later lanes: CAO `#131/#78/#164`; gptme has mostly design lanes; OpenACP current bugs are covered/fixed. Recommended `WATCH`. |
| Duplicate/PR risk | CAO has high overlap with `#115/#251`; OpenACP `#247` covered by `#251`; gptme is low open-PR but issue lanes are stale/design-heavy. Recommended `WATCH`. |
| Process/community gates | Upstream comment/PR is not process-safe without one exact issue, duplicate search and repro. Recommended `WATCH`. |
| Runner/repro feasibility | Secret-free unit repro is feasible for all three, but no selected test card yet; CT `216` is Hermes-only. Recommended `WATCH`. |
| Actionability synthesis | Keep all three verify-first. Promote CAO as main follow-up lane, not candidate. No upstream action. |

## Candidate lanes to watch

### awslabs/cli-agent-orchestrator

Primary follow-up lane:

- `#131` inbox stuck / brittle status detection, with duplicate gate over `#115` and `#251`.
- `#78` terminal prompt not returned after inbox delivery, only if fresh terminal capture/repro proves current-main failure.
- `#164` duplicate delivery race, only after event-driven overlap is understood.

Promotion gate: exact issue-level duplicate search, current-main runner checkout, secret-free unit repro around inbox/tmux/status formatting or state transition, then targeted regression test. Do not use live provider auth as baseline evidence.

### gptme/gptme

Keep as passive low-noise terminal-agent watch.

- Avoid broad design lanes `#554/#607/#216/#523` until maintainer direction narrows scope.
- `#1591/#2193` are only gap-check/comment-first lanes after verifying current PR/test coverage.
- Best future target shape: small CLI/tooling/session regression with existing unit tests and no provider secret requirement.

Promotion gate: current-main repro on Python/Poetry runner, `make test` or targeted test command, plus duplicate/closed-PR search for the exact subsystem.

### Open-ACP/OpenACP

Keep as ecosystem/protocol watch.

- Do not touch `#247` while PR `#251` is open.
- Do not treat `#240/#232` as open candidates without verifying adapter commit/version state.
- Reopen only for fresh session/protocol bug with no adapter-side fix.

Promotion gate: Node `>=20` / pnpm `10.5+` runner, targeted Vitest/TypeScript check, no real bot tokens.

## 3-role critique

No upstream comment/PR is being sent, so this is an internal critique:

- Factology/duplicates: do not claim low PR count means free lane; CAO and OpenACP strongest issues are covered by active PRs/commits.
- Process gates: all three need exact issue + repro + tests; CAO additionally requires AWS-style recent PR/issue sweep before significant work.
- Actionability: exactly one status is selected: `WATCH`. The unblock event is a single exact issue plus runner-backed repro and duplicate gate.

## Runner

Runner was not used in this cycle. Reason: repo/issue/PR scouting only; no issue reached `CANDIDATE` or status-changing `COMMENT-FIRST`. Dedicated `corp-opensource-runner` is required before upstream evidence. CT `216` remains Hermes-specific and was not used.

## Decision

- `awslabs/cli-agent-orchestrator`: strongest follow-up lane, still `WATCH`.
- `gptme/gptme`: passive low-noise terminal-agent watch.
- `Open-ACP/OpenACP`: protocol/ecosystem watch; no current PR candidate.

No upstream comment or PR from this cycle.
