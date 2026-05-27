# Browser-agent repo scope, 2026-05-27

Tracker: [#70](https://github.com/serejaris/corp-opensource/issues/70)
Umbrella: [#52](https://github.com/serejaris/corp-opensource/issues/52)
Decision time: 2026-05-27 21:32 UTC
Final `next_status`: `WATCH`

## Scope

Bounded browser-agent / browser automation slice for Paperclip-adjacent runtime scouting.

| Repo | Fit | Live gate |
|---|---|---|
| `browser-use/browser-use` | Browser automation for AI agents; high-value browser action, CDP/session, MCP and trace/debug surface. | MIT; default `main`; `95866` stars / `10789` forks; `66` open issues / `179` open PRs; latest release `0.12.9` published `2026-05-26T03:45:22Z`; pushed `2026-05-26T03:46:14Z`. |
| `browserbase/stagehand` | SDK for browser agents; strong action/extract/observe, CUA, server and runtime compatibility surface. | MIT; default `main`; `22828` stars / `1541` forks; `91` open issues / `135` open PRs; latest release `stagehand-server-v3/v3.7.0` published `2026-05-27T19:48:00Z`; pushed `2026-05-27T19:43:50Z`. |
| `Skyvern-AI/skyvern` | Browser workflow/RPA automation and self-hosted/cloud service stack; useful reference for browser-service reliability. | AGPL-3.0; default `main`; `21756` stars / `2025` forks; `34` open issues / `123` open PRs; latest release `v1.0.36` published `2026-05-10T02:06:38Z`; pushed `2026-05-27T21:03:04Z`. |

Required skills note: local skills `open-source-bug-scouting` / `open-source-pr-workflow` are not installed as callable skills in this session, so this cycle used the documented fallback: parent GitHub live gates plus 6-subagent scouting. No upstream comment/PR was attempted.

## Parent live gates

### browser-use/browser-use

- Community profile: health `75`, MIT license, README and CONTRIBUTING link to docs.
- Test/repro gate: Python `>=3.11,<4.0`; pyproject has pytest timeout, ruff and pyright config; many browser/CDP/cloud lanes require a browser runtime or API keys.
- High duplicate pressure:
  - `#4846` MCP CDP state/screenshot/list-tabs issue overlaps open PRs `#4881/#4847/#4849`.
  - `#4801` icon-only accessibility issue overlaps open PR `#4880`.
  - strict dependency pin issues `#4824/#4877/#4868` overlap open PRs `#4882/#4878`.
  - trace/audit trail `#4860` overlaps open PR `#4927`.
  - CDP/session stability lanes overlap `#4862/#4875/#4858`.

### browserbase/stagehand

- Community profile: health `50`, MIT license, README and PR template; no standalone CONTRIBUTING found.
- Test/repro gate: pnpm `9.15.0`, Node `^20.19.0 || >=22.12.0`, turbo scripts for `core/e2e/server/evals/cli`; Browserbase/cloud paths can require credentials.
- High duplicate pressure:
  - `#2148` headed-mode focus steal overlaps `#2162`.
  - `#1996` debug always enabled overlaps `#2156/#2166/#2083`.
  - `#2055` Cloudflare Workers runtime issue overlaps `#2062`.
  - `#2046/#2035` image MIME hardcode overlaps `#2159/#2048`.
  - verifier/eval direction has active PR series `#2132-#2138`.
- Many external contributor PRs are `external-contributor:awaiting-approval`, so queue collision risk is high.

### Skyvern-AI/skyvern

- Community profile: health `87`, AGPL-3.0, CONTRIBUTING, Code of Conduct, PR template and security docs.
- Contribution gate: latest-version check, issue search, detailed repro steps; `needs-repro` blocks action; security issues must not be public.
- Test/repro gate: Python/Node/Docker/Postgres/browser/service stack; pre-commit hooks expected. Local/self-hosted bugs require a dedicated runner, not CT `216`.
- High duplicate/support pressure:
  - `#6044` OpenCode OAuth/API-key setup overlaps open PR `#6100`.
  - `#5898` per-run BYOK overlaps open PR `#5899`.
  - recording/artifact route lanes overlap `#6167/#6189`.
  - `#6004/#5756/#4666` are support/setup/platform-heavy; no clean minimal patch surface yet.

## 6-subagent synthesis

| Role | Result |
|---|---|
| Repo fit/activity | Ranking: Stagehand as best future SDK/runtime lane, browser-use as highest-popularity fit but noisy, Skyvern as useful reference with license/service-stack caveat. Recommended `WATCH`. |
| Issue quality/actionability | Best theoretical lanes are Stagehand `#1996/#2046/#2035`, Skyvern `#6146`, browser-use `#4846`, but all need duplicate gates; many are already covered. Recommended `WATCH`. |
| Duplicate/PR risk | Strongest lanes across all three repos are covered by open PR clusters. Recommended `WATCH`. |
| Process/community gates | Upstream comment/PR is not process-safe without one exact issue, duplicate search and repro. Recommended `WATCH`. |
| Runner/repro feasibility | Secret-free local tests are possible for browser-use/stagehand subsets, but no selected current-main repro card exists; Skyvern requires heavier service stack. Recommended `WATCH`. |
| Actionability synthesis | Record repo-level browser-agent slice as `WATCH`; next steps are post-merge gap scans, not PR hunting. |

## Follow-up lanes

### browser-use/browser-use

- Post-merge gap scan after `#4880/#4881/#4882/#4927`.
- Reopen only if a remaining gap is secret-free and testable on local browser/CDP/MCP.
- Avoid cloud-only failures unless runner has secret-managed Browser Use credentials.

### browserbase/stagehand

- Best future repo for this slice, but wait for queue resolution around `#2055/#1996/#2046/#2035/#2148`.
- Candidate promotion requires clean non-overlap with `#2162/#2166/#2156/#2062/#2159/#2048`.
- Use core/server/local-browser tests first; Browserbase/eval credentials only via runner env.

### Skyvern-AI/skyvern

- Watch only for exact self-hosted/local quickstart/browser-service bugs with current-main runner evidence.
- Avoid cloud/account/support issues and AGPL-sensitive broad feature work.
- `#6146` is too vague until error body/repro steps are available; `#6044/#5898` are covered by PRs.

## 3-role critique

No upstream comment/PR is being sent, so this is an internal critique:

- Factology/duplicates: do not overclaim any issue as uncovered; most best lanes are already covered by PR clusters.
- Process gates: stagehand external PR queue and Skyvern needs-repro process make unsolicited PRs risky; browser-use needs exact local test evidence.
- Actionability: exactly one status is selected: `WATCH`. Unblock event is a single exact issue after post-merge gap scan plus runner-backed repro.

## Runner

Runner was not used in this cycle. Reason: repo/issue/PR scouting only; no issue reached `CANDIDATE` or status-changing `COMMENT-FIRST`. Dedicated `corp-opensource-runner` is required before upstream evidence. CT `216` remains Hermes-specific and was not used.

## Decision

Keep all three repos in scope, but do not open upstream comments or PRs now.

- `browserbase/stagehand`: strongest future lane after external PR queue resolution.
- `browser-use/browser-use`: high-value post-merge gap watch.
- `Skyvern-AI/skyvern`: reference/watch with AGPL and service-stack caution.
