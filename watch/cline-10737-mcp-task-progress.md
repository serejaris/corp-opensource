# Cline MCP `task_progress` duplicate triage

## Upstream

- Issue: https://github.com/cline/cline/issues/10737
- Internal issue: https://github.com/serejaris/corp-opensource/issues/17
- Upstream triage comment: https://github.com/cline/cline/issues/10737#issuecomment-4551559976
- Repo: `cline/cline`
- Status checked: 2026-05-27 02:13 -03

## Signal

The issue reports that Cline forwards internal `task_progress` into Jira MCP tool arguments. Strict MCP servers validate against their schema and reject the unexpected field with a Pydantic `ValidationError`.

This is in scope because it is a tool/MCP boundary bug in a coding-agent runtime.

## Duplicate Cluster

Subagents found the same bug class in:

- https://github.com/cline/cline/issues/8256
- https://github.com/cline/cline/issues/9684
- https://github.com/cline/cline/issues/9919
- https://github.com/cline/cline/issues/10426

Relevant open PRs:

| PR | State | Notes |
|---|---|---|
| https://github.com/cline/cline/pull/9951 | open, conflicting | Minimal `UseMcpToolHandler` strip; stale since 2026-03-24; one Ubuntu e2e failure/cancelled smoke. |
| https://github.com/cline/cline/pull/9542 | open, conflicting | Similar minimal strip; stale since 2026-02-27; old checks green but branch conflicts. |
| https://github.com/cline/cline/pull/8589 | open, mergeable | Claimed fix, but review says implementation did not actually filter `task_progress`; JetBrains workflow comments reported failures. |

Live search for `10737 OR task_progress OR UseMcpToolHandler OR mcpHub.callTool` returned no new clean PR beyond the known cluster.

## Local Checkout Note

Attempted clone to `/Users/ris/Documents/GitHub/cline-10737` failed checkout because `git-lfs` is not installed:

```text
git-lfs filter-process: git-lfs: command not found
fatal: the remote end hung up unexpectedly
warning: Clone succeeded, but checkout failed.
```

Do not use that broken checkout as evidence of repo state until `git-lfs` is installed and checkout is restored.

Source files were later readable after disabling the LFS filter for a restore command, but the checkout index still reports a broken delete/untracked state. Treat source reads as source-only evidence, not as a clean working tree.

A clean source checkout was then created at `/Users/ris/Documents/GitHub/cline-10737-clean` with LFS filters disabled at clone time:

```bash
git -c filter.lfs.process= -c filter.lfs.required=false -c filter.lfs.clean= -c filter.lfs.smudge= clone https://github.com/cline/cline.git /Users/ris/Documents/GitHub/cline-10737-clean
```

Status:

```text
## main...origin/main
```

## Current Main Source Audit

Checked source on 2026-05-27 from the clean no-LFS checkout:

- `apps/vscode/src/core/task/tools/handlers/UseMcpToolHandler.ts:67` parses `block.params.arguments` into `parsedArguments`.
- `apps/vscode/src/core/task/tools/handlers/UseMcpToolHandler.ts:168` passes `parsedArguments` directly to `config.services.mcpHub.callTool(server_name, tool_name, parsedArguments, config.ulid)`.
- No source-side deletion of `parsedArguments.task_progress` exists in this handler.
- Prompt text still tells the model that `task_progress` is supported by every tool call, including `use_mcp_tool`, while also saying it must be a separate parameter and not inside `arguments`.

This confirms the bug class still appears present on current readable source, but does not by itself justify a fresh PR because the duplicate PR queue is already messy.

## PR Comparison

- `#9951`: adds guarded `delete parsedArguments.task_progress`, but targets old `src/core/task/tools/handlers/UseMcpToolHandler.ts`; current repo path is `apps/vscode/src/core/task/tools/handlers/UseMcpToolHandler.ts`, so the PR is now conflicting/stale.
- `#9542`: same minimal handler idea, also old path/conflicting.
- `#8589`: mergeable, but its current diff only adds `.changeset/mcp-task-progress-filter.md`; it does not apply the behavior.

Posted upstream triage comment on `#10737` with this comparison and stated that a fourth PR should wait for maintainer preference.

## Decision

`DUPLICATE TRIAGE / no fresh PR yet`.

The bug is real and likely patchable, but a fresh PR would add another branch to an already messy queue. Next useful actions:

- compare `#9951` vs current `main` once a clean checkout exists;
- if maintainers want a fresh PR, make it minimal, with a unit test around MCP-bound arguments;
- otherwise comment on the existing canonical PR/issue with duplicate comparison.

## Proposed Test Shape

When safe to implement:

- exercise `UseMcpToolHandler.execute`;
- feed `block.params.arguments='{"issue_key":"X","task_progress":[...]}'`;
- mock `mcpHub.callTool`;
- assert forwarded MCP arguments exclude `task_progress`;
- assert focus/task progress tracking still uses the native block-level field.
