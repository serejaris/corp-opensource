# Cycle 12 - PR-readiness rules for AI-native repos

Дата проверки: 2026-05-27.

Метод: роль 6 из scouting loop - PR-readiness. Для уже найденных кандидатов также сверены duplicate/assignment gates и live PR/issue state через `gh`.

## Итог

| Repo | External PR allowed | Gate | Required proof/tests | Lane |
|---|---:|---|---|---|
| `cline/cline` | Yes | Feature/significant work needs approved issue/discussion; small bugfixes allowed directly | `npm test`, `npm run format`, `npm run lint`; focused PR and test procedure | Ready-PR for small bugfix. Current PR `#11087` open/mergeable, checks green |
| `anomalyco/opencode` | Yes | Issue-first for all PRs; conventional title; template compliance; non-compliant PRs can auto-close after 2h | Local test evidence; `bun` dev flow; generated SDK when API/SDK changes | Ready-PR only with linked issue. Current PR `#29530` open/mergeable, standards/compliance green |
| `CopilotKit/CopilotKit` | Yes | Reach out/file issue first for significant work; fork workflow; branch naming recommended | `pnpm install`, `pnpm build`; docs update for functionality | Ready-PR for scoped bug with issue. Current PR `#5035` open/mergeable; Vercel previews need team auth |
| `browser-use/browser-use` | Yes | Contribution guide favors single-focus PRs; issues/Discord for work selection; blank issues disabled | `uv sync --all-extras --dev`, `./bin/lint.sh`, `./bin/test.sh`; demo screenshot/gif | Comment/watch now: best issue `#4796` already covered by PR `#4815` with green CI |
| `OpenHands/OpenHands` | Yes | Fork PRs accepted; big changes should be discussed in issue/Slack; PR must have `HUMAN:` note and readiness confirmation for first-time contributors | `make build`, `make run`; PR template requires human test evidence and How to Test | Comment-first for current issue `#14563`; needs maintainer direction on global skills/container semantics |
| `OpenHands/software-agent-sdk` | Yes | Same draft/human-testing culture; issue number and end-to-end proof expected | E2E proof before/after for bugfix; repo has precommit/tests/eval workflows | Current PR `#3394` open/mergeable but review decision still `CHANGES_REQUESTED`; wait maintainer/check refresh |
| `pydantic/pydantic-ai` | Yes, but heavily gated | Non-trivial PR requires maintainer-agreed issue and assignment; unassigned/duplicate PRs can be closed by PR Guard | `make`; lint/mypy/docs/tests/coverage matrix; AI-generated code checkbox | Do not open cold PR. Current `#5678`/`#5680` green/mergeable; wait maintainer. New work comment-first/assignment-first |
| `e2b-dev/E2B` | Yes | Minimal CONTRIBUTING; practical gate is CI/CLA/review | Package-specific SDK tests; generated files/codegen check | Current PR `#1354` open/mergeable, review required; CLA/action statuses need follow-up before it is truly merge-ready |
| MCP Python SDK | Yes | All PRs need issue except trivial; wait maintainer feedback or `ready for work`; comment for assignment | `uv run pytest`, `uv run pyright`, `uv run ruff check .`, `uv run ruff format .`, README snippets if needed | Comment-first unless trivial/doc fix |
| MCP TypeScript SDK | Yes | Features/significant changes need issue; ready labels matter; comment for assignment | `pnpm build:all`, `pnpm test:all`, `pnpm lint:all`; target `main` or `v1.x` correctly | Ready-PR only for straightforward tested bugfix; otherwise comment-first |
| MCP Go SDK | Yes | Significant changes need issue; non-Help Wanted issues should wait for confirmation; proposals needed for API/dependency changes | `go test ./...`, `gofmt`, `go vet`, `staticcheck`, race CI | Ready-PR for small bug/test; comment-first for API/design |
| MCP Rust SDK | Likely yes | No CONTRIBUTING found; CI enforces conventional commits, fmt, clippy, tests, semver checks | `cargo +nightly fmt --all -- --check`, `cargo clippy --all-targets --all-features -- -D warnings`, `cargo test --all-features` | Comment-first until contribution policy/issue acceptance is explicit |
| `aaif-goose/goose` | Yes | Start small; link existing issue; use draft only for useful discussion; address AI reviewer comments; avoid many PRs in burst | Hermit setup; `cargo check`, `cargo test`, `cargo fmt`, `cargo clippy --all-targets -- -D warnings` | Comment-first for `#9136`/`#9332`; both open/unassigned; no PR before assignment/direction |
| `trycua/cua` | Yes | Issue-first recommended; formatting/pre-commit expected | `uv run black .`, `uv run isort .`, `uv run ruff check --fix .`, `uv run mypy .`, `uv run pre-commit run`; package CI | Comment-first/repro-first for `#1724`; `#1725` needs Windows smoke before PR |
| `langchain-ai/deepagents` | Yes, but strict | External PR must use `Fixes #N` and PR author must be assigned to linked issue; otherwise auto-close. Fork PR from `main/master` auto-closes. English-only. | From package root: `make format`, `make lint`, `make test`; title `TYPE(SCOPE): DESCRIPTION` | Comment-first/assignment-first only. Current PR `#3616` auto-closed by assignment gate |

## Ready-PR lanes

1. `cline/cline`: small provider/runtime bugfix lane is open when scoped and tested. Existing `#11087` is already in this lane and green.
2. `anomalyco/opencode`: good ready-PR lane only after issue link and template compliance. Existing `#29530` is clean by current gates.
3. `CopilotKit/CopilotKit`: scoped bugfix with issue is acceptable. Existing `#5035` waits on review/auth, not code readiness.
4. MCP TypeScript/Go SDKs: small tested bugfixes can be PR-first-ish; anything API/spec-related is comment-first.

## Comment-first / assignment-first lanes

1. `pydantic/pydantic-ai`: assignment-first for non-trivial work. Existing PRs should be watched, not multiplied.
2. `langchain-ai/deepagents`: assignment gate is hard. Do not open new external PR until assigned.
3. `aaif-goose/goose`: good fit, but current issues are unassigned and design/runtime-sensitive.
4. `OpenHands/OpenHands`: main repo issues are duplicate/design-heavy. Use comments unless a maintainer gives a concrete small patch path.
5. `trycua/cua`: needs platform repro before code PR.
6. MCP Python SDK: issue and `ready for work` gate is explicit.
7. MCP Rust SDK: no contribution doc found; use issue/comment first.
8. `browser-use/browser-use`: current best bug is duplicate-covered; review/comment only unless a new uncovered failing trace appears.

## Additional Cycle 12 candidates

| Candidate | PR-readiness decision |
|---|---|
| `pydantic/pydantic-ai#5387` | Best no-secret regression candidate from repro-path pass, but Pydantic rules still make this assignment/comment-first before PR unless maintainer has already agreed. |
| `pydantic/pydantic-ai#5606` | Split candidate; profile/settings half may be small, but non-trivial provider semantics require issue alignment first. |
| `pydantic/pydantic-ai#5282` | Comment-first because Temporal dynamic toolset instruction semantics may need maintainer design direction. |
| `langchain-ai/deepagents#3050` | Design-first; shell path rewriting can be a footgun and deepagents assignment gate blocks cold external PRs. |
| `langchain-ai/deepagents#3573` | Watch/duplicate-race; no fresh PR unless assignment gap opens. |
| `anomalyco/opencode#15226` | Promising low-duplicate provider request-shape bug. Ready-PR lane only after local repro and linked issue-compliant PR body. |
| `microsoft/playwright-mcp#1635` | Low-duplicate browser/MCP candidate, but needs maintainer confirmation of target repo before PR. |
| `modelcontextprotocol/python-sdk#2687` | Clean repro, but MCP Python rules require issue confirmation/assignment or `ready for work` before PR. |
| `openai/codex#24725` | Watch/access-sensitive; upstream PR path is restricted, so useful action is maintainer-facing evidence/comment. |
| `openai/codex#23131` | No fresh PR lane; reporter has patch branch and external PR path is restricted. |

## Source files checked

- `browser-use/browser-use`: `.github/CONTRIBUTING.md`, docs contribution guide, docs local setup, `.github/ISSUE_TEMPLATE/config.yml`, CI on `#4815`.
- `OpenHands/OpenHands`: `CONTRIBUTING.md`, `.github/pull_request_template.md`, `.github/workflows/pr-readiness-confirm.yml`, issue `#14563`.
- `OpenHands/software-agent-sdk`: `CONTRIBUTING.md`, `.github/PULL_REQUEST_TEMPLATE.md`, PR `#3394`.
- `cline/cline`: `CONTRIBUTING.md`, `.github/pull_request_template.md`, PR `#11087`.
- `anomalyco/opencode`: `CONTRIBUTING.md`, `.github/pull_request_template.md`, `.github/workflows/compliance-close.yml`, `.github/workflows/pr-standards.yml`, PR `#29530`.
- `CopilotKit/CopilotKit`: `CONTRIBUTING.md`, `.github/PULL_REQUEST_TEMPLATE.md`, PR `#5035`.
- `pydantic/pydantic-ai`: `CONTRIBUTING.md`, `.github/pull_request_template.md`, `.github/workflows/pr-guard.yml`, PRs `#5678`, `#5680`, `#5681`.
- `e2b-dev/E2B`: `CONTRIBUTING.md`, generated files and SDK test workflows, PR `#1354`.
- MCP SDKs: Python/TypeScript/Go `CONTRIBUTING.md`; Go PR template; Rust CI and repo tree.
- `aaif-goose/goose`: `CONTRIBUTING.md`, `.github/pull_request_template.md`, `.github/workflows/take.yml`, issues `#9136`, `#9332`.
- `trycua/cua`: `CONTRIBUTING.md`, `Development.md`, issues `#1724`, `#1725`.
- `langchain-ai/deepagents`: `.github/PULL_REQUEST_TEMPLATE.md`, `.github/workflows/require_issue_link.yml`, `.github/workflows/reopen_on_assignment.yml`, `.github/workflows/block_fork_main_prs.yml`, PR `#3616`.
