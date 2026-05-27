# Morph SDK generic scope closure, 2026-05-27

Tracker: [#64](https://github.com/serejaris/corp-opensource/issues/64)

Umbrella: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Final `next_status`: `NO-GO` for generic AI-native scouting scope.

This is a narrow `NO-GO`: it closes generic Morph SDK scouting as stale/low-signal. It does not block a future Morph-specific lane if a concrete current-main bug or user request appears.

## Scope

Combined check for:

- `morph-labs/morph-python-sdk`
- `morph-labs/morph-typescript-sdk`

The question was whether these SDK repos should remain in the general Paperclip-like / AI-native scouting queue.

## Parent Live Gates

### `morph-labs/morph-python-sdk`

- Repo metadata: created `2025-12-28`, default branch `main`, not archived, not fork.
- Activity: pushed/released `2026-04-06`; latest release `v0.1.111`.
- Public surface at check time: `3` stars, `0` forks, `0` open issues.
- Open PR queue: one open PR, [`#8`](https://github.com/morph-labs/morph-python-sdk/pull/8) `SDK: snapshot TTL support`, `CONFLICTING`.
- Duplicate/stale signal: [`#9`](https://github.com/morph-labs/morph-python-sdk/pull/9) `Add snapshot TTL support to the Python SDK` was merged on `2026-04-01` shortly after `#8`, making `#8` look superseded.
- Issue signal: no open issues. Recent closed issue `#5` was closed with docs/fresh-issue guidance rather than a current public bug lane.
- Community/profile: `25%`; README and Apache-2.0 license present; no `CONTRIBUTING`, issue template, or PR template.
- Test/package surface: `pyproject.toml`, `tests/unit`, `tests/integration`, `pytest` dev deps. This does not prove runner-ready repro without a specific bug card and secrets gate.
- Cloud/secrets: README requires Morph Cloud account/API key for real SDK use.

### `morph-labs/morph-typescript-sdk`

- Repo metadata: created `2024-12-17`, default branch `main`, not archived, not fork.
- Activity: pushed `2026-03-05`; latest GitHub release `v0.0.12`, published `2025-06-26`.
- Public surface at check time: `3` stars, `4` forks, `3` open issues.
- Open issues: [`#8`](https://github.com/morph-labs/morph-typescript-sdk/issues/8), [`#9`](https://github.com/morph-labs/morph-typescript-sdk/issues/9), [`#10`](https://github.com/morph-labs/morph-typescript-sdk/issues/10); all old, unlabeled, and last updated by maintainer replies on `2025-06-11` saying docs/source were updated.
- Open PR queue: old/stale/conflict-heavy. PRs `#12`-`#18` are open; most are `CONFLICTING`. `#15` is `MERGEABLE` but broad feature/parity scope.
- Community/profile: `37%`; README and Apache-2.0 license present; no `CONTRIBUTING`, issue template, or PR template.
- Test/package surface: `package.json` has `test`, `test:integration`, module format tests, Jest/ts-jest/tsup. Integration paths likely need Morph Cloud API state.
- Cloud/secrets: README requires Morph Cloud API key for real SDK use.

## 6-Subagent Synthesis

1. Repo fit/activity: Morph Cloud as a domain is adjacent to agent runtime/devbox infrastructure, but these public SDK repos are low-surface and not strong popular targets.
2. Issue quality: no current candidate. Python has no open issue lane; TypeScript issues are stale and already have maintainer replies.
3. PR queue/duplicates: Python `#8` looks superseded by merged `#9`; TypeScript queue is stale/conflict-heavy and overlaps around SDK parity/features.
4. Process/license/security: Apache-2.0 is clean, but contribution process is weak: no contribution docs/templates and low community profile.
5. Runner/repro/secrets: do not run repro now. There is no `PR-READY` bug card, dedicated runner is not confirmed, CT `216` is Hermes-only, and real SDK checks likely require Morph Cloud account/API key.
6. Actionability: close generic scope as `NO-GO`; keep only a Morph-specific revisit condition.

## 3-Subagent Critique

- Factology/duplicates: pass with corrections. Do not overclaim a legal/provenance blocker; both repos are Apache-2.0. Record Python PR count as one open PR plus small PR history, not only total metadata.
- Process gates: pass. 6-subagent scouting, parent live gates, negative runner gate, and no-upstream-action gate are recorded.
- Actionability: split between `WATCH` and `NO-GO`; final choice is `NO-GO` for generic scope only. Any future watch is reference-only and Morph-specific.

## Runner Gate

Runner skipped.

Reason: no `PR-READY`, no reproducible current-main bug card, cloud/account/API-key risk, and no dedicated `corp-opensource-runner` availability confirmation. CT `216` is Hermes-only.

## Decision

`next_status: NO-GO` for generic Morph SDK scouting.

No upstream comments, PRs, or repro runs.

Revisit only with:

- Morph-specific request or fresh public issue;
- current-main repro with clear expected/actual behavior;
- clean linked/competing PR gate;
- maintainer signal if the issue is feature/parity/process-sensitive;
- unchanged Apache-2.0/license gate;
- runner-ready repro plan without private secrets in repo;
- for cloud checks: host-managed secrets or runner environment, documented env vars, and a minimal scenario that avoids billing/production state.
