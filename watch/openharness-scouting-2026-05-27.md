# HKUDS/OpenHarness scouting, 2026-05-27

Live state: parent `gh` pass на 2026-05-27 19:57-20:01 UTC.

Upstream: [`HKUDS/OpenHarness`](https://github.com/HKUDS/OpenHarness).
Internal tracker: [`corp-opensource#61`](https://github.com/serejaris/corp-opensource/issues/61).
Umbrella tracker: [`corp-opensource#52`](https://github.com/serejaris/corp-opensource/issues/52).

Upstream actions в этом блоке: 0 PR, 0 comments.

Final `next_status`: `WATCH`.

Required skills `open-source-bug-scouting` и `open-source-pr-workflow` в текущей session недоступны, поэтому использован documented fallback: 6 read-only scouting subagents, parent live gates, затем 3-subagent critique.

## Parent Live Gates

| Gate | Result |
|---|---|
| Repo state | `HKUDS/OpenHarness`: 13195 stars, 2180 forks, default branch `main`, not archived, not fork, latest release [`v0.1.9`](https://github.com/HKUDS/OpenHarness/releases/tag/v0.1.9) published `2026-05-07T11:10:46Z`, pushed `2026-05-27T10:19:24Z`, updated `2026-05-27T19:26:25Z`. |
| Queue pressure | 11 open issues, 14 open PRs. Recent merged PRs include [`#279`](https://github.com/HKUDS/OpenHarness/pull/279), security-titled command/session isolation fixes [`#276`](https://github.com/HKUDS/OpenHarness/pull/276), [`#272`](https://github.com/HKUDS/OpenHarness/pull/272), [`#261`](https://github.com/HKUDS/OpenHarness/pull/261), [`#258`](https://github.com/HKUDS/OpenHarness/pull/258), and memory/runtime work [`#266`](https://github.com/HKUDS/OpenHarness/pull/266), [`#267`](https://github.com/HKUDS/OpenHarness/pull/267). |
| Product fit | High Paperclip-like / AI-native fit: Python agent harness with tools, skills, memory, provider flows, permission/governance, multi-agent coordination, and Ohmo channel agent runtime. |
| Issue queue | Open issues are mostly feature/discussion, not reproducible bug reports. Stronger leads are feature/design lanes: [`#47`](https://github.com/HKUDS/OpenHarness/issues/47) Cursor Agent CLI support, [`#221`](https://github.com/HKUDS/OpenHarness/issues/221) Azure Entra provider, [`#18`](https://github.com/HKUDS/OpenHarness/issues/18) run-level evidence archive. |
| Bug search | Targeted searches for open bug/error/crash/fail/Windows/MCP/tool-call lanes returned no open actionable bug. Closed bug/security issues show maintainers often absorb narrow fixes directly with tests. |
| PR / duplicate pressure | Open PR queue is conflict-heavy. `#93`, `#34`, and parts of `#273` appear already absorbed or partly covered in main. Memory lane `#250/#72/#68` overlaps recent `#266/#267`. UI/Windows/vision lane `#277` overlaps prior vision, TUI, and Windows work. |
| Process gate | `CONTRIBUTING.md` and PR template exist. Expected validation: `uv run ruff check src tests scripts`, `uv run pytest -q`, and `cd frontend/terminal && npx tsc --noEmit` if frontend touched. User-visible changes need `CHANGELOG.md` under `Unreleased`. |
| License / security | License is MIT. No visible `SECURITY.md`; do not publish security-sensitive details without a separate private-channel decision. |
| Runner gate | No runner repro in this repo-level scan. Any future candidate around sandbox, subprocess, filesystem permissions, MCP stdio/http, TUI, or long-running agent loops needs dedicated runner validation. |

## 6-Subagent Synthesis

| Role | Finding |
|---|---|
| repo-fit | Strong active watch target: 13k+ stars, fresh pushes, direct AI-agent harness fit. Risks: very young repo, high churn, single-maintainer concentration, and mixed-quality PR backlog. |
| issue triage | No clean bug candidate. Best leads are `#221`, `#47`, and `#18`, but all are feature/design or discussion lanes and need source/repro/design gates before any upstream comment. |
| PR / duplicate pressure | Several open PRs are stale/conflicting or partly absorbed into main. Memory, provider, Windows, UI, and workflow areas need per-lane duplicate gates before status promotion. |
| process gates | Contribution path is clear for small tested PRs, but there is no selected bug, no regression card, and no runner evidence. Security reporting path is not explicit. |
| repro / runner | Core checks are secret-free: `uv run pytest -q`, `uv run ruff check src tests scripts`, targeted pytest files, frontend typecheck. Live provider/channel tests require secrets and should not be default evidence. |
| PR-readiness | Repo-level final should be `WATCH`. Promote only after a concrete open bug or narrow current-main code-path finding with repro and duplicate gate. |

## 3-Subagent Critique

| Role | Result |
|---|---|
| factology / duplicates | `WATCH` is correct. Do not overclaim low competition or duplicate-clean areas. Open issues are feature/discussion; no selected bug target, regression card, or fail-before evidence. |
| process gates | Repo-level scan does not satisfy issue-level gates. Open issues lack a selected candidate; runner/repro is missing; contribution docs require tests and changelog for visible changes. |
| actionability | Do not comment-first on `#221/#47/#18` now. Any comment would be low-signal without source audit, MRE, or resolving overlap such as whether `#247` actually covers `#18`. |

## Decision

Keep `HKUDS/OpenHarness` as active verify-first harness watch, not a current upstream target.

Final `next_status`: `WATCH`.

Promotion gates:

1. Pick exactly one future issue-level lane, preferably `#47` if current-main source check proves a concrete `OPENHARNESS_TEAMMATE_COMMAND` wrapper incompatibility.
2. Run fresh issue/PR duplicate search and timeline check for that lane.
3. Build a secret-free MRE and targeted test card in `corp-opensource-runner`.
4. Only after new evidence, decide between `COMMENT-FIRST` and `CANDIDATE`; do not open PR without fail-before/pass-after evidence or maintainer direction.
