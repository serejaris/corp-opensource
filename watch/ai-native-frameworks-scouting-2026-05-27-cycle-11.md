# Cycle 11 / Repo-fit scouting

Дата проверки: 2026-05-27 06:48 -03.

Метод: 6 ролей — repo-fit, bug-signal, repro-path, patchability, duplicate-race, PR-readiness. Фокус: Paperclip/Hermes/Codex-like workflows — coding agents, harnesses, MCP/tool execution, browser/computer-use sandboxes, eval/replay/test harnesses, memory/skills/workflows.

## Top 5 repos

| Rank | Repo | Score | Lane | Почему |
|---:|---|---:|---|---|
| 1 | [e2b-dev/E2B](https://github.com/e2b-dev/E2B) | 11 | `PR-READY / blocked by CLA + runner repro queue` | Sandbox/runtime fit, release `e2b@2.25.0` on 2026-05-27, recent external merge [#1342](https://github.com/e2b-dev/E2B/pull/1342), active bug [#1352](https://github.com/e2b-dev/E2B/issues/1352), our [#1354](https://github.com/e2b-dev/E2B/pull/1354) already red/green but CLA-gated. |
| 2 | [pydantic/pydantic-ai](https://github.com/pydantic/pydantic-ai) | 10 | `WATCH / no new duplicate while #5678/#5680/#5681 pending` | Best test/maintainer surface; release `v1.103.0` on 2026-05-27; external fixes merged [#5188](https://github.com/pydantic/pydantic-ai/pull/5188), [#5571](https://github.com/pydantic/pydantic-ai/pull/5571); rich current bug stream around tools/providers/serialization. |
| 3 | [CopilotKit/CopilotKit](https://github.com/CopilotKit/CopilotKit) | 9 | `WATCH / current PR review; next target should be harness/protocol gap` | AG-UI/tool runtime fit, release `v1.58.0` on 2026-05-26, external fixes/harness PRs merged [#5025](https://github.com/CopilotKit/CopilotKit/pull/5025), [#5028](https://github.com/CopilotKit/CopilotKit/pull/5028); active bugs around frontend tools/HITL/runtime events. |
| 4 | [langchain-ai/deepagents](https://github.com/langchain-ai/deepagents) | 8 | `COMMENT-FIRST / assignment-gated but high harness fit` | Direct agent harness scope; release `deepagents==0.6.4` on 2026-05-26; active bug labels include `external`; strong unit/integration/eval tests under `libs/deepagents`. Current [#3587](https://github.com/langchain-ai/deepagents/issues/3587) remains assignment/reopen lane after closed PR [#3616](https://github.com/langchain-ai/deepagents/pull/3616). |
| 5 | [trycua/cua](https://github.com/trycua/cua) | 8 | `CANDIDATE / needs platform repro before PR` | Best CUA/computer-use fit; recent CUA driver/test-harness work [#1727](https://github.com/trycua/cua/pull/1727), [#1720](https://github.com/trycua/cua/pull/1720), active Windows/macOS driver bugs [#1725](https://github.com/trycua/cua/issues/1725), [#1706](https://github.com/trycua/cua/issues/1706). Weakness: sampled recent bugfix merges were collaborator-heavy, not clear external merge lane. |

## Evidence snapshot

- `e2b-dev/E2B`: 12,373 stars, 27 open issues, 34 open PRs; pushed 2026-05-27; release `e2b@2.25.0`.
- `pydantic/pydantic-ai`: 17,333 stars, 382 open issues, 178 open PRs; pushed 2026-05-27; release `v1.103.0`.
- `CopilotKit/CopilotKit`: 31,790 stars, 318 open issues, 188 open PRs; pushed 2026-05-27; release `v1.58.0`.
- `langchain-ai/deepagents`: 23,414 stars, 136 open issues, 46 open PRs; pushed 2026-05-27; release `deepagents==0.6.4`.
- `trycua/cua`: 17,131 stars, 127 open issues, 208 open PRs; pushed 2026-05-27; release `cua-driver-v0.2.0`.

## Likely test commands

| Repo | Commands |
|---|---|
| E2B | `pnpm test --recursive --if-present`; package-specific SDK tests after checkout. |
| pydantic-ai | `make test`; targeted `uv run pytest path/to/test.py`; `make lint`. |
| CopilotKit | `pnpm test`; targeted `nx run <project>:test`; `pnpm lint`; `pnpm check-format`. |
| deepagents | `cd libs/deepagents && make test`; targeted `uv run --group test pytest -n auto -vvv tests/unit_tests/...`; `make integration_test` only after repro is stable. |
| trycua/cua | `uv run ruff check .`; `uv run mypy .`; `uv run black .`; `uv run isort .`; platform driver tests need macOS/Windows-capable repro. |

## PR-readiness appendix

Live check: 2026-05-27 via GitHub API, repo docs, PR templates, workflow lists, and branch protection contexts.

| Repo | Lane | Hard gate | Exact local checks / commands | GitHub required contexts seen |
|---|---|---|---|---|
| `OpenHands/OpenHands` | `READY-PR` for small bug/docs; `COMMENT-FIRST` for core agent/product changes | Fork PR; conventional PR title; explain issue/why; `How to Test` required; template says keep draft until ready and has a human-tested checkbox | Setup: `make build`; run: `make run`; broad checks: `make lint`, `make test`; frontend: `cd frontend && npm run lint`, `npm run test`; backend lint through `poetry run pre-commit ...` | `Enterprise Python Unit Tests (3.12)`, `Lint enterprise python`, `Lint frontend`, `Lint python`, `Python Tests on Linux (3.12)` |
| `browser-use/browser-use` | `READY-PR` only with tight focused bug + demo; otherwise `COMMENT-FIRST` because docs say maintainers are overwhelmed | Focused PR; include screenshot/gif/demo; pass CI; blank issues disabled | Setup: `uv sync --all-extras --dev`; helper checks: `./bin/lint.sh`, `./bin/test.sh`; examples: `uv run examples/simple.py`; pyproject uses `ruff`, `pyright`, `pytest` | `code-style`, `syntax-errors`, `type-checker` |
| `anomalyco/opencode` | `COMMENT-FIRST / ISSUE-FIRST`; fresh PR only after issue exists and maintainer has not claimed it | All PRs must reference an existing issue; comment to take issue, maintainer may assign; UI/core features require design review; PR template required or auto-rejected; avoid long AI-generated text | Setup: Bun 1.3+, `bun install`, `bun dev`; checks: `bun run lint`, `bun run typecheck`; do not run root `bun test` because root script exits; package-specific tests needed | Branch `dev` protected, but API exposed no required contexts; workflows include `test.yml`, `typecheck.yml`, `pr-standards.yml`, `compliance-close.yml` |
| `cline/cline` | `READY-PR` for small bug/typo/type/doc; `COMMENT-FIRST / APPROVED-ISSUE` for features or functional changes | All contributions start with GitHub issue unless small bug/typo/minor wording/simple type fix; PRs without approved issues may be closed; link issue/discussion; include test procedure | VS Code app: `cd apps/vscode && npm run install:all`, `npm run protos`, `npm run test`, `npm run format:fix`, `npm run lint`, `npm run test:e2e` when relevant; SDK: `cd sdk && bun run build`, `bun test`, `bun run check` | Branch protected, but API exposed no required contexts; workflows include `ext-vscode-test.yml`, `ext-vscode-test-e2e.yml`, `sdk-test.yml` |
| `CopilotKit/CopilotKit` | `COMMENT-FIRST` for significant work; small bug PR possible but issue/reach-out preferred | Contributing and PR template both say reach out before significant feature work; file issue first; fork workflow; branch naming `docs/`, `feat/`, `fix/<ISSUE_NUMBER>-...`; allow edits by maintainers | Setup: `pnpm install`; build: `pnpm build`; dev: `pnpm dev`; checks: `pnpm run lint`, `pnpm run check-format`, `pnpm run check-types`, `pnpm run test`, `pnpm run test:coverage`, `pnpm run check:packages` as relevant | `ESLint`, `Prettier`, `Test (18.x)`, `TypeScript`, `Test (20.x)` |
| `e2b-dev/E2B` | `READY-PR` mechanically, but `NEEDS-REPRO` for sandbox/runtime bugs | CONTRIBUTING is minimal: open PR/issue/Discord discussion; no PR template found; branch protected but no contexts exposed | Root package: `pnpm test --recursive --if-present`, `pnpm --if-present --recursive run lint`, `pnpm --if-present --recursive run typecheck`, `pnpm --if-present --recursive run format`; generated files: `make generate`, `make generate-js`, `make generate-python` | Branch protected, but API exposed no required contexts; workflows include `cli_tests.yml`, `js_sdk_tests.yml`, `python_sdk_tests.yml`, `sdk_tests.yml`, `generated_files.yml`, `lint.yml`, `typecheck.yml` |
| `modelcontextprotocol/python-sdk` | `COMMENT-FIRST / ASSIGNMENT-GATED`; only trivial typo/docs can skip | All PRs require corresponding issue unless trivial; wait for maintainer feedback or `ready for work`; comment before starting so maintainers can assign; PRs without linked issue/buy-in may be closed | Setup: `uv sync --frozen --all-extras --dev`; checks: `uv run pytest`, `uv run pyright`, `uv run ruff check .`, `uv run ruff format .`; snippets if docs changed: `uv run scripts/update_readme_snippets.py`; optional `pre-commit run --all-files` | Branch protected, but API exposed no required contexts; workflows include `main.yml`, `conformance.yml`, `shared.yml`, `zizmor.yml` |
| `modelcontextprotocol/typescript-sdk` | `READY-PR` for straightforward few-line bugfix with tests; `COMMENT-FIRST / ASSIGNMENT-GATED` for features/significant changes | Open issue before features/significant changes; issues with `needs confirmation`, `needs repro`, `needs design` are not ready; comment before work for assignment; branch target differs: `main` for new features/v2, `v1.x` for v1 bugfixes | Setup: `pnpm install`; build: `pnpm build:all`; tests: `pnpm test:all`; lint: `pnpm lint:all`; check: `pnpm check:all`; conformance: `pnpm run test:conformance:all` when protocol behavior changes | Branch protected, but API exposed no required contexts; workflows include `main.yml`, `conformance.yml`, `deploy-docs.yml`, `update-spec-types.yml` |

PR-readiness takeaway: safest fresh PR lanes are small, regression-backed bugs in `OpenHands`, `browser-use`, `E2B`, and trivial/straightforward `modelcontextprotocol/typescript-sdk`. `opencode`, `modelcontextprotocol/python-sdk`, and most nontrivial `cline`/`CopilotKit` work are comment-first or assignment-gated. Before code, rerun duplicate PR search and create/update the regression card.

## Six-role synthesis

- Repo-fit: strongest fit is E2B/trycua for sandbox and computer-use, pydantic-ai/CopilotKit/deepagents for tool/runtime/harness boundaries.
- Bug-signal: E2B #1352 and trycua #1725/#1706 are the clearest infra bugs; pydantic-ai has high-volume provider/tool bugs but several are already occupied; CopilotKit has frontend-tool protocol bugs; deepagents has external agent-tool bugs.
- Repro-path: E2B needs sandbox runner; trycua needs platform repro; pydantic-ai/CopilotKit/deepagents are most fixture-friendly locally.
- Patchability: pydantic-ai/CopilotKit/deepagents have best targeted test surface; E2B depends on live sandbox/SDK behavior; trycua can become expensive if driver/platform-specific.
- Duplicate-race: do not duplicate pydantic-ai #5678/#5680/#5681, CopilotKit #5035, deepagents #3616/#3587, E2B #1354. For trycua, rerun fresh PR/issue linked scan immediately before code.
- PR-readiness: best immediate non-code action is E2B CLA; best next scouting action is deepagents/CopilotKit/pydantic targeted issue scan after active PRs settle; trycua only after platform repro.

## Near misses / exclude for now

- [openai/codex #24725](https://github.com/openai/codex/issues/24725): strong regression-first candidate for plugin-namespaced skills exceeding `MAX_NAME_LEN`; internal tracker [#41](https://github.com/serejaris/corp-opensource/issues/41). Regression card is clear, but `openai/codex` external PR creation is invitation-only, so lane is `WATCH / comment-first`, not ready PR.
- [openai/codex #23131](https://github.com/openai/codex/issues/23131): high-impact TypeScript SDK JSONL parser bug with reporter patch available, but the reporter already supplied compare branch + validation and cannot open PR because upstream restricts PR creation. No duplicate PR action for us; watch for maintainer mirror/apply.
- [OpenHands/OpenHands](https://github.com/OpenHands/OpenHands): excellent repo-fit, but current [#14563](https://github.com/OpenHands/OpenHands/issues/14563) is maintainer-direction-needed and SaaS/self-hosted semantics are too easy to get wrong.
- [anomalyco/opencode](https://github.com/anomalyco/opencode): very active and strong Codex-like fit, but current actionable lane is already [#29530](https://github.com/anomalyco/opencode/pull/29530) watch; root `test` script explicitly says not to run tests from root.
- [browser-use/browser-use](https://github.com/browser-use/browser-use): great browser-agent fit, but recent top candidates are already duplicate-covered or dependency-noise; keep as watch/review only.
- [cline/cline](https://github.com/cline/cline): huge fit, but duplicate-race remains high across MCP/tool/retry issues.
- MCP SDKs: important protocol layer, but current high-signal issues are comment-first or duplicate-covered in cycle 9/10.

## Next actions

1. Sign E2B CLA for `@serejaris`, then comment `@cla-bot check` on [E2B #1354](https://github.com/e2b-dev/E2B/pull/1354).
2. Use issue [corp-opensource#40](https://github.com/serejaris/corp-opensource/issues/40) as the Cycle 11 queue.
3. Track [Codex #24725](https://github.com/openai/codex/issues/24725) in [corp-opensource#41](https://github.com/serejaris/corp-opensource/issues/41), but do not open PR without maintainer signal because upstream is invitation-only for external PRs.
4. If runner becomes available, prioritize E2B #1352 and trycua #1725 repro. Without runner/platform repro, keep both in watch/candidate, not PR.
5. After active pydantic/CopilotKit/deepagents PR queues settle, run a fresh duplicate scan before selecting a new regression-first candidate.
