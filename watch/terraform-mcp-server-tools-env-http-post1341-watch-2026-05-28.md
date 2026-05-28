# Terraform MCP Server tools/env HTTP watch, 2026-05-28 13:41 UTC

Контекст: bounded scouting pass после tracker-only heartbeat `2026-05-28T13:39:48Z`; tracker synthesis: [#52 comment](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4564626726). Обязательные skills `open-source-bug-scouting` / `open-source-pr-workflow` в этой среде недоступны, поэтому использован documented fallback: 2 read-only scouting subagents из-за `agent thread limit reached`, parent live GitHub gates, 2-of-3 critique subagents plus parent actionability critique. Upstream actions: `0`. Runner actions: `0`.

## Scouting synthesis

- Fresh discovery поднял новый material issue-level сигнал: `hashicorp/terraform-mcp-server#376`.
- Backlog refresh сохранил `spring-ai#6197`, `zed#57155`, `probe#568`, `claude-peers-mcp#64`, `Agent-S#195`, `vercel/ai#15652`, `gemini-cli#27503`, `spring-ai#6196`, `apify#88` без перехода в `PR-READY`.
- Parent duplicate/race gates не нашли direct issue duplicate или open PR cover для `#376`.
- Process critique подтвердил, что issue-first уже достаточен, но PR/comment без независимого runner-backed Docker repro преждевременен.
- Actionability critique parent-side: минимальный repro должен быть Docker/current-main smoke around env-var streamable HTTP mode and tool filtering; no `PR-READY` without fail-before evidence.

## Parent live gates

### `hashicorp/terraform-mcp-server#376`

- Issue: [`hashicorp/terraform-mcp-server#376`](https://github.com/hashicorp/terraform-mcp-server/issues/376), open, created/updated `2026-05-28T13:41:16Z`.
- Labels: none; assignee: none; comments: none at gate time.
- Repo: [`hashicorp/terraform-mcp-server`](https://github.com/hashicorp/terraform-mcp-server), public, MPL-2.0, about 1.38k stars, default branch `main`, pushed `2026-05-28T13:23:55Z`.
- Report: `--tools` and `--toolsets` are silently ignored when the server is started in streamable-HTTP mode via `TRANSPORT_MODE` env vars. The env-var path exposes the default `toolsets=all`, including write/destructive Terraform tools, even when only a small read/tool subset was requested.
- Repro in issue: Docker image `hashicorp/terraform-mcp-server:0.5.2`, `TRANSPORT_MODE=streamable-http`, `MCP_SESSION_MODE=stateless`, `--tools=list_workspaces,get_workspace_details`; `tools/list` returns 43 tools and startup logs `Enabled toolsets: [all]`.
- Root-cause hypothesis in issue: `main()` enters `shouldUseStreamableHTTPMode()` before `rootCmd.Execute()`, so cobra persistent flags are read before flag parsing. This matches the symptom: `toolsets` falls back to `all` and `tools` falls back to empty.

### Duplicate / PR race

- Targeted issue search for `TRANSPORT_MODE --tools` found only `#376`.
- Targeted issue/PR searches for `tools toolsets environment variable streamable http`, `streamable-http tools toolsets`, and `TRANSPORT_MODE --tools` found no open PR cover.
- Critique noted older merged toolset/filtering PRs `#215` and `#242` as provenance only, not blockers.
- Weak race note: open `#348` may touch server bootstrap via official MCP Go SDK, but it is not a direct cover for env-var flag parsing.

### Process gate

- Discoverable root files show `LICENSE` and `README.md`; root `CONTRIBUTING.md` / `SECURITY.md` were not found via parent gate.
- CODEOWNERS/process critique points ownership at `@hashicorp/team-proj-mcp-servers`.
- Bug template expectations are effectively covered by `#376`: version/image, description, actual/expected, testing plan, context, root-cause pointer.
- No discoverable DCO/CLA requirement found in repo/workflows during critique.
- PR template has security-control / rollback checklist implications; any future PR must explicitly discuss that the bug broadens the exposed MCP tool surface to write/destructive actions.
- CI expectations from critique: Go `1.26.3`, `make test` / `go test -v ./...`, preferably `make test-e2e` or Docker smoke, and changelog handling unless maintainers apply `no-changelog-needed`.

## Decision

`next_status: CANDIDATE`

`hashicorp/terraform-mcp-server#376` is a compact, high-fit MCP/tool-surface regression candidate with a concrete Docker repro and no direct PR cover. It is not `COMMENT-FIRST` or `PR-READY` yet: we need independent dedicated-runner evidence that the env-var streamable HTTP path ignores parsed flags on current main or the released image, plus a repeat duplicate/PR gate and full 3-role critique before any upstream comment/PR.

Runner status unchanged: `corp-opensource-runner` is still unavailable via `#10`; CT216 remains Hermes-only and was not used.
