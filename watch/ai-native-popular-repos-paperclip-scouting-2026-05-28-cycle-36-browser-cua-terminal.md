# Fresh browser / CUA / terminal repo discovery, 2026-05-28 03:26 UTC

Цикл: bounded fresh discovery по popular AI-native browser/CUA/terminal repos.

Required skills `open-source-bug-scouting` / `open-source-pr-workflow` в этом окружении недоступны, поэтому использован documented fallback: parent live GitHub gates, 6 read-only roles in two waves, затем internal critique. Upstream action count: `0`.

`next_status: CANDIDATE`

No upstream comment, PR, ping, rerun, rebase, or runner repro was made.

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560610697).

Runner gate: `corp-opensource-runner` недоступен из этого окружения; ни один lane не имеет current-main fail-before evidence. Поэтому нет `PR-READY`.

## Search inputs

GitHub search queries:

- `mcp agent stars:>1000 pushed:>2026-05-01`
- `browser agent automation stars:>1000 pushed:>2026-05-01`
- `coding agent cli stars:>1000 pushed:>2026-05-01`
- `computer use agent stars:>500 pushed:>2026-05-01`

Parent shortlist after de-dup against existing scope: `nanobrowser/nanobrowser`, `ntegrals/openbrowser`, `magnitudedev/browser-agent`, `simular-ai/Agent-S`, `earendil-works/pi`, `browserwing/browserwing`.

Factology correction: one subagent confused `ntegrals/openbrowser` with `openbrowser/openbrowser`; parent live gates are authoritative. `ntegrals/openbrowser` is real but issue-disabled / PR-heavy. `openbrowser/openbrowser` was not selected.

Factology correction: one subagent drifted from `earendil-works/pi` into unrelated `pydantic/pydantic-ai` numbering. Parent live gates are authoritative: `earendil-works/pi#5055/#5065/#5095` are open and assigned to `mitsuhiko`.

## Repo gates

| Repo | Live gate | Decision |
|---|---|---|
| `simular-ai/Agent-S` | `11661` stars, Apache-2.0, pushed `2026-05-13`, `21` issues / `9` PRs. | Active `WATCH`; selected lane `#195` is internal `CANDIDATE`. |
| `nanobrowser/nanobrowser` | `13049` stars, Apache-2.0, pushed `2025-11-24`, `44` issues / `22` PRs. | Active browser-agent `WATCH`; reserve lane `#270`, while `#291` is duplicate-covered by PR `#318`. |
| `magnitudedev/browser-agent` | `4067` stars, Apache-2.0, pushed `2026-02-08`, `26` issues / `5` PRs. | Passive `WATCH`; reserve lane `#154`; process is weaker due lower freshness / no visible contribution docs. |
| `browserwing/browserwing` | `1283` stars, MIT, pushed `2026-05-13`, `18` issues / `1` PR. | Active-light `WATCH`; candidate-like reserve `#18`, comment-first `#23/#19`; avoid security lanes `#11/#12`. |
| `earendil-works/pi` | `56445` stars, MIT, pushed `2026-05-27`, `36` issues / `12` PRs. | Broad AI-infra/TUI `WATCH`; not primary for this browser/CUA cycle because current lanes are maintainer-assigned. |
| `ntegrals/openbrowser` | `9457` stars, MIT, pushed `2026-04-02`, issues disabled, PR backlog `#18-#37`. | `NO-GO` for new work in this cycle: no issue-backed lane and high PR/process risk. |

## Lane decisions

| Lane | Live result | Decision |
|---|---|---|
| `simular-ai/Agent-S#195` | Open, unassigned, no labels; no exact PR found for `WindowsACI.hotkey()` splitting `enter` into characters. | Primary `CANDIDATE` / best future runner target. Minimal regression: `WindowsACI.hotkey("enter")` and other single keys must be one key token; multi-key chords must still work. Pure unit/string-generation evidence may be enough before any Windows GUI smoke. |
| `simular-ai/Agent-S#196` | Open, unassigned; exact open PR `#197` says `Closes #196` for shell interpolation in permission dialogs. | `WATCH / duplicate-covered`; security-sensitive, no new PR/comment. |
| `nanobrowser/nanobrowser#291` | Open with `bug` + `needs-triage`; exact open PR `#318` fixes GLM/manual JSON extraction. | `WATCH / duplicate-covered`; monitor `#318`, no duplicate. |
| `nanobrowser/nanobrowser#270` | Open `bug`, no assignee; no exact PR found. Comment suggests Chrome 142 / `puppeteer-core@24.29.0` may fix `Passed function cannot be serialized`. | Reserve `WATCH / CANDIDATE-after-runner`; needs Chrome/MV3 extension current-main repro and duplicate check around DOM/extension PRs. |
| `magnitudedev/browser-agent#154` | Open, no labels/assignee; no exact PR found. `keyboard:type` does not trigger Vue filter/search input events. | Reserve `WATCH / CANDIDATE-after-runner`; needs Playwright/Patchright + Vue filterable-select fixture. |
| `magnitudedev/browser-agent#155` | Open, security-sensitive `spawn(command, { shell: true })` config injection claim; no exact PR found. | `COMMENT-FIRST / NO-GO for PR`; public patch is risky without maintainer stance/security process. |
| `browserwing/browserwing#18` | Open, labels `bug`, `compatibility`, `confirmed`; Chinese issue about OpenClaw garbling. No exact PR found. | `WATCH / CANDIDATE-after-repro`; upstream language should be Chinese if commenting. |
| `browserwing/browserwing#23` | Open `bug`, Chinese issue about system proxy not read. | `COMMENT-FIRST`; needs repro clarification and proxy fixture. |
| `browserwing/browserwing#11/#12` | Security-labeled network/browser MCP surface. | `NO-GO` without explicit maintainer/security process signal. |
| `earendil-works/pi#5055/#5065/#5095` | All open and assigned to maintainer `mitsuhiko`; no exact PR found by parent search. | `WATCH`; technically testable but process/assignment gate blocks cold action. |
| `ntegrals/openbrowser` | Issues disabled; open PRs include security-ish `#37` and many PR-only topics. | `NO-GO`; no clean issue-backed target. |

## 6-role synthesis

Selected lane: `simular-ai/Agent-S#195`.

Reason: small deterministic CUA primitive bug, no exact PR, low security risk, and a bounded test contract. This is not `PR-READY` because no current-main repro/regression was run in a runner.

Reserve lanes: `magnitudedev/browser-agent#154`, `nanobrowser#270`, and `browserwing#18`. These are useful but heavier: browser fixture, Chrome extension lifecycle, or language/repro constraints.

Duplicate-covered/no-go lanes: `nanobrowser#291 -> #318`, `Agent-S#196 -> #197`, `ntegrals/openbrowser` issue-disabled PR backlog, and `browserwing#11/#12` security lanes.

## Critique

- Factology: use parent gates over subagent drift for `ntegrals/openbrowser` and `earendil-works/pi`.
- Process: no upstream action; exact-covered/security/process-gated lanes stay watch/no-go.
- Actionability: update internal watch/scope/tracker only. Next concrete unblock is runner-backed `Agent-S#195` unit/current-main repro.
