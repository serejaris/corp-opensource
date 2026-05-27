# Goose #9332: `PR_SET_PDEATHSIG` kills stdio MCP subprocesses

Checked at `2026-05-27T08:35Z`.

## Upstream

- Issue: [aaif-goose/goose#9332](https://github.com/aaif-goose/goose/issues/9332)
- Prior behavior PR: [aaif-goose/goose#8242](https://github.com/aaif-goose/goose/pull/8242)
- Internal issue: [serejaris/corp-opensource#34](https://github.com/serejaris/corp-opensource/issues/34)

## Decision

`COMMENT-FIRST / wait maintainer direction`.

The bug signal is strong and no duplicate fix PR was found, but the patch direction is design-sensitive because it trades off two valid lifecycle goals:

- avoid orphaned MCP subprocesses after goose exits;
- avoid killing live stdio MCP subprocesses when a Tokio worker thread exits.

## Six-Agent Findings

- Repo-fit: active, relevant, but maintainers have not commented on #9332.
- Bug-signal: strong; stdio MCP dies between tool calls with `Transport closed`.
- Repro-path: Linux-only; macOS cannot verify the current subprocess cleanup test.
- Patchability: naive removal is small but likely contentious; robust dedicated spawn thread/runtime is larger.
- Duplicate-race: no open/closed PR covers #9332; #8242 is regression source, not a fix.
- PR-readiness: DCO signoff, conventional commit PR title, small focused PR expected.

## Regression Shape

Add a Linux-only regression in `crates/goose/tests/subprocess_cleanup.rs`:

1. Spawn a long-lived child through `tokio::process::Command + configure_subprocess`.
2. Do the spawn from a separate `std::thread`.
3. Let the spawning thread exit while the parent process remains alive.
4. Assert the child remains alive.

Expected current behavior on Linux: the child dies because `PR_SET_PDEATHSIG` is tied to the spawning thread, not the parent process.

## Upstream Comment

Posted maintainer-alignment comment before coding:

https://github.com/aaif-goose/goose/issues/9332#issuecomment-4552827471

## Next Action

Wait for maintainer direction, or run a Linux repro on `corp-opensource-runner` once available and prepare a draft regression-only PR if maintainers ask for evidence first.
