# OpenClaw/OpenClaude identity-provenance scan, 2026-05-27

Tracker: [#63](https://github.com/serejaris/corp-opensource/issues/63)

Umbrella: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Final `next_status`: `NO-GO`

## Scope

Combined identity/provenance check for:

- `openclaw/openclaw`
- `Gitlawb/openclaude`

This was not a bugfix candidate cycle. It was a verify-first lane left by the cycle 29 repo-card overlay.

## Parent Live Gates

### `openclaw/openclaw`

- Repo metadata: created `2025-11-24`, default branch `main`, not archived, not fork.
- Activity: pushed/updated on `2026-05-27`; latest release `openclaw 2026.5.26` / `v2026.5.26`, published `2026-05-27`.
- Scale at check time: about `375k` stars, `78k` forks, `3.8k` issues, `3.0k` PRs.
- License: GitHub reports `Other/NOASSERTION`; local `LICENSE` text is MIT and points to `THIRD_PARTY_NOTICES.md`.
- Community/profile: `README`, `CONTRIBUTING.md`, `LICENSE`, PR template; no code of conduct in community profile.
- Issue/PR samples: very high-volume queue with fast automation/human follow-up, proof/readiness labels, and high duplicate-race risk.

Interpretation: these facts do not prove the repo is noncanonical. They are sufficient risk signals to require external canonical validation and manual maintainer/process proof before treating the repo as a contribution target.

### `Gitlawb/openclaude`

- Repo metadata: created `2026-04-01`, default branch `main`, not archived, not fork.
- Activity: pushed `2026-05-27`; latest release `v0.15.0`, published `2026-05-26`.
- Scale at check time: about `27.9k` stars, `8.6k` forks, `91` issues, `120` PRs.
- Community/profile: `README`, `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `LICENSE`, PR template; community profile `100%`.
- PR sample: many open mergeable PRs without labels; no clean contribution lane selected.
- Issue sample: support/gateway/provider problems, duplicates/overlaps, and noisy user reports; no safe candidate bug selected.
- License/provenance: `LICENSE` explicitly says the repository contains code derived from proprietary Anthropic Claude Code CLI and that the project does not have authorization to distribute the proprietary source; MIT is only for contributor modifications where legally permissible.

Interpretation: this is a confirmed provenance/license blocker for corp-opensource contribution work.

## 6-Subagent Synthesis

1. Identity/provenance/activity: combined recommendation `NO-GO`; OpenClaw needs canonical validation, OpenClaude remains verify-only with provenance concerns.
2. Issue quality/noise: OpenClaw issue flow is too automation-heavy and race-prone; OpenClaude issue quality does not yield a safe candidate.
3. PR queue/competition: OpenClaw is saturated and duplicate-prone; OpenClaude has a large unlabeled mergeable PR queue.
4. Process/license/security: OpenClaw has process docs but metadata/license ambiguity; OpenClaude has an explicit proprietary-derived-code blocker.
5. Runner/repro/secrets: no runner repro should run because no trusted candidate bug exists and provenance is the blocking gate.
6. Actionability: final status should be `NO-GO`, not `WATCH`, because the blocker is trust/provenance rather than ordinary missing repro.

## 3-Subagent Critique

- Factology/provenance: pass with correction. Do not state that OpenClaw is proven noncanonical; record only risk signals and required external canonical validation. OpenClaude license/provenance blocker is confirmed by the license text.
- Process gates: pass. 6-subagent scouting, parent live gates, negative runner gate, and no-upstream-action gate are recorded.
- Actionability: pass. `NO-GO` has a concrete unblock condition and no upstream action should be taken.

## Runner Gate

Runner skipped.

Reason: there is no `PR-READY` lane, no safe candidate bug card, and the blocking gate is provenance/canonicality. CT `216` is Hermes-only and must not be used for this general open-source check. Dedicated `corp-opensource-runner` availability would not change the provenance gate.

## Decision

`next_status: NO-GO`

No upstream comments, no PR, no runner repro.

Revisit only if all of these are true:

- external canonical source / maintainer identity validation;
- legally clean license and provenance;
- normal maintainer process without mass generated/race loop;
- one manual current-main reproducible bug card;
- no linked or competing PR;
- dedicated runner readiness without private secrets in repo.
