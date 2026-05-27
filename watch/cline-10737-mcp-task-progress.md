# Cline MCP `task_progress` duplicate triage

## Upstream

- Issue: https://github.com/cline/cline/issues/10737
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
