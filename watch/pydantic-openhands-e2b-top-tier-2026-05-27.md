# Pydantic AI / OpenHands / E2B top-tier watch, 2026-05-27

Tracker: [#71](https://github.com/serejaris/corp-opensource/issues/71)
Umbrella: [#52](https://github.com/serejaris/corp-opensource/issues/52)
Decision time: 2026-05-27 21:52 UTC
Final `next_status`: `WATCH`

## Scope

Bounded top-tier repo-scope recheck for the highest-fit AI-native/Paperclip-adjacent repos that were still marked `promote` in repo cards.

| Repo | Fit | Live gate |
|---|---|---|
| `pydantic/pydantic-ai` | Typed Python agent framework; strong provider/tool/MCP/UI adapter/evals/OTel surface with good unit-test culture. | MIT; default `main`; `17344` stars / `2141` forks; `384` open issues / `181` open PRs; latest release `v1.103.0` published `2026-05-27T02:37:05Z`; pushed `2026-05-27T21:08:19Z`. |
| `OpenHands/OpenHands` | Developer-agent product/control-plane: skills, browser preview, runtime/sandbox, resolver, LLM endpoints and self-hosting. | GitHub license key `other`, pyproject says MIT; default `main`; `75060` stars / `9519` forks; `164` open issues / `202` open PRs; latest release `1.7.0` published `2026-05-01T14:52:47Z`; pushed `2026-05-27T20:57:10Z`. |
| `e2b-dev/E2B` | Agent sandbox/runtime and SDK layer; useful for command, reconnect, stdout, upload and rate-limit reliability. | Apache-2.0 at repo level; default `main`; `12375` stars / `923` forks; `26` open issues / `31` open PRs; latest release `@e2b/python-sdk@2.25.0` published `2026-05-27T21:33:49Z`; pushed `2026-05-27T21:33:53Z`. |

Required skills note: local skills `open-source-bug-scouting` / `open-source-pr-workflow` are not installed as callable skills in this session, so this cycle used the documented fallback: parent GitHub live gates plus 6-subagent scouting. No upstream comment/PR was attempted.

## Parent live gates

### pydantic/pydantic-ai

- Community profile: health `75`, license MIT, CONTRIBUTING, Code of Conduct, issue template, PR template and README are present.
- Test/repro gate: Python `>=3.10`; `uv` workspace; pytest/ruff/pyright config. Many provider/MCP/UI adapter bugs can be unit-tested secret-free with mocks/local objects.
- Active duplicate clusters:
  - `#5688` MCPToolset `http_client` / FastMCP `follow_redirects` crash is covered by open PR `#5694`.
  - `#5671` Google cached content request shape is covered by open PR `#5681`.
  - `#5669` Mistral usage preservation is covered by open PR `#5693`.
  - `#5679` TextContent metadata round-trip is already our open PR `#5680`.
  - `#5666` serialize_any bytes preservation is already our open PR `#5678`.
- Process gate: pydanty/maintainer automation and same-day bug PR flow make cold duplicate PRs risky. Use assignment-first or post-merge gap scans only.

### OpenHands/OpenHands

- Community profile: health `87`, CONTRIBUTING, Code of Conduct, issue template, PR template and README are present.
- Test/repro gate: Python `>=3.12,<3.14`, Poetry/uv migration surface, Docker, Playwright/browser and frontend/runtime paths. Unit-only fixes are possible, but product/runtime lanes usually need dedicated runner evidence.
- Active duplicate/process clusters:
  - `#14563` global skills not loaded in CLI/Docker already has our comment-first asking whether maintainers prefer Settings/personal-skills repo path or narrow CLI/self-hosted mount option. Wait for maintainer direction.
  - `#14476` conversation metadata race overlaps PRs `#14579/#14582` and older nearby work.
  - `#14523` env propagation overlaps PRs `#14531/#14535`.
  - `#14569` webbrowser/app-tab issue is near draft PR `#14568` and broader port/app-surface requests.
- Process gate: small linked fixes are possible, but core agent/runtime/product semantics need maintainer direction. No new upstream action in this cycle.

### e2b-dev/E2B

- Community profile: health `62`, Apache-2.0 repo license, CONTRIBUTING, Code of Conduct, issue template, PR template and README are present.
- Test/repro gate: Python SDK uses Python `^3.10`, pytest/ruff/ty; JS SDK uses Node `>=20.18.1`, pnpm/vitest/tsc. SDK unit tests can be secret-free, but sandbox lifecycle bugs usually require E2B credentials in runner-managed env.
- Active duplicate/process clusters:
  - `#1352` resumed sandbox `commands.connect` stops delivering stdout after a large response is the best future lead. It has a Linear link `ENG-4146`; no direct open PR found via `stdout connect resumed sandbox` search.
  - `#1325` Retry-After/rate-limit signal is covered by PRs `#1334/#1353`.
  - `#1259` async stdout/stderr callback wait boundary overlaps PR `#1261` and older wait/iteration work.
  - `#888` `py.typed` is covered by PR `#1298`.
- Process gate: E2B is the best pragmatic follow-up repo in this slice, but `#1352` needs runner-backed current-main repro and likely maintainer/Linear ownership check before comment or PR.

## 6-subagent synthesis

| Role | Result |
|---|---|
| Repo fit/activity | Ranking: `pydantic/pydantic-ai` strongest strategic fit, `e2b-dev/E2B` best pragmatic sandbox/infra follow-up, `OpenHands/OpenHands` highest product relevance but noisiest. Recommended `WATCH`. |
| Issue quality/actionability | `e2b-dev/E2B#1352` is the strongest future lead; `OpenHands#14563` is already comment-first; pydantic's best narrow lanes are covered. Recommended `WATCH`. |
| Duplicate/PR risk | pydantic and OpenHands have very high same-day collision risk; E2B is better but current SDK lanes still overlap PRs/internal Linear. Recommended `WATCH`. |
| Process/community gates | Upstream action is acceptable only after exact repro and non-overlap proof. Pydantic is assignment-first; OpenHands needs maintainer direction for product/runtime semantics; E2B is process-lightest but has CLA/internal tracking. Recommended `WATCH`. |
| Runner/repro feasibility | pydantic has the best secret-free unit path, but current lanes are covered; OpenHands and E2B need dedicated runner for runtime/sandbox evidence. Recommended `WATCH`. |
| Final synthesis | No safe new upstream lane now; record watch and use post-merge/runner triggers. Recommended `WATCH`. |

## Follow-up lanes

### pydantic/pydantic-ai

- Keep top active watch for new small regressions in providers, MCP, UI adapters, OTel and evals.
- Do not race current PR clusters: `#5694`, `#5681`, `#5693`, `#5680`, `#5678`.
- Promotion trigger: fresh issue with no open PR, failing unit fixture, assignment/maintainer signal, and a regression card.

### OpenHands/OpenHands

- Keep `#14563` in wait state after our comment-first. Do not open a skills mount/config PR until maintainers choose the direction.
- Watch app/browser/runtime issues only after active PRs settle and a local/runner repro exists.
- Avoid broad product/RFC issues and support-only reports.

### e2b-dev/E2B

- Promote `#1352` only after dedicated `corp-opensource-runner` repro with E2B token kept outside the repo, SDK version matrix, public template, region and control case.
- Before comment/PR, re-check Linear/maintainer ownership and PR search across `connect`, `stdout`, `resume`, `stream`, `wait`.
- Post-merge gap scan `#1325/#1334/#1353` only if Retry-After behavior remains broken.

## 3-role critique

No upstream comment/PR is being sent, so this is an internal critique:

- Factology/duplicates: do not treat pydantic `#5688` as open for us now; `#5694` exists. Do not treat E2B `#1325` as open; `#1334/#1353` exist. OpenHands `#14563` already has our question, so more comments would be noise.
- Process gates: pydantic requires assignment-first discipline, OpenHands needs maintainer/product direction for runtime semantics, and E2B may already be internally tracked through Linear.
- Actionability: exactly one status is selected: `WATCH`. Unblock event is either maintainer direction on `OpenHands#14563`, runner-backed E2B `#1352` evidence, or a fresh pydantic small regression after current PR clusters settle.

## Runner

Runner was not used in this cycle. Reason: repo/issue/PR scouting only; no issue reached `CANDIDATE` or status-changing `COMMENT-FIRST`. Dedicated `corp-opensource-runner` is required before E2B/OpenHands runtime evidence. CT `216` remains Hermes-specific and was not used.

## Decision

Keep all three repos in scope, but do not open upstream comments or PRs now.

- `pydantic/pydantic-ai`: top strategic watch, post-merge gap scan only.
- `e2b-dev/E2B`: best next runner-backed follow-up, with `#1352` as lead-after-repro.
- `OpenHands/OpenHands`: high-value product watch, wait for maintainer direction on `#14563`.
