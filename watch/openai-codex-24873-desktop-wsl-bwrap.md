# OpenAI Codex #24873 Desktop WSL bwrap sandbox

Дата: 2026-05-28

Upstream: https://github.com/openai/codex/issues/24873

Trackers:

- Umbrella: https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561378941

Статус: `CANDIDATE`

Upstream actions: `0`

Runner actions: `0`

## Коротко

`openai/codex#24873` reports that Codex Desktop on WSL2 cannot start sandboxed tool execution because the WSL-distributed `codex` binary does not find system `bwrap` and has no adjacent bundled `codex-resources/bwrap`. The reporter also checked that `/usr/bin/bwrap` exists and works outside Codex. Enabling `use_legacy_landlock = true` changes the failure into a permission-profile panic, so the fallback is not a usable workaround for this environment.

This is a strong runtime/tool-execution lead because it breaks minimal commands before execution. It is not `PR-READY`: the bug is Desktop WSL packaging/path-specific and needs WSL/Codex Desktop evidence, not just Linux source inspection.

## Parent live gates

- Issue: open, unassigned, labels absent, comments absent at gate time, created `2026-05-28T06:21:43Z`.
- Repo: `openai/codex`, ~86k stars, active `main`, license not detected by GitHub API.
- Reporter environment: Codex Desktop WSL binary under `/mnt/c/Users/<WindowsUser>/.codex/bin/wsl/<hash>/codex`, `codex-cli 0.133.0`, WSL2 Ubuntu 20.04, kernel `5.15.167.4-microsoft-standard-WSL2`.
- Failure shape: trivial sandboxed command fails before running with "bubblewrap is unavailable"; `use_legacy_landlock = true` fails with a permission-profile incompatibility panic.
- Related prior art: `#21915` is closed and close-adjacent, but it was for the VS Code extension / remote Linux surface on `0.130.0-alpha.5`; it does not cover Desktop WSL cache layout or the Landlock fallback panic.
- Duplicate/PR search: no exact open issue or PR cover found for `#24873`, Desktop WSL `bwrap` discovery, missing `codex-resources/bwrap`, or the Landlock permission-profile fallback. Open PRs around permissions/sandboxing are adjacent only.
- Runner: `corp-opensource-runner` remains unavailable via tracker `#10`; current runner would also need Windows host + WSL2 + Codex Desktop for full evidence.

## Repro plan

Minimum repro needs a Windows host with WSL2 and Codex Desktop:

1. Verify system `bwrap`: `which bwrap`, `bwrap --version`, and a minimal `bwrap --ro-bind / / --dev /dev --proc /proc --tmpfs /tmp /bin/true`.
2. Start a Codex Desktop WSL session with normal workspace sandboxing.
3. Run a trivial command such as `pwd` and capture the panic plus `RUST_BACKTRACE=1`.
4. Inspect the adjacent WSL binary resource layout, especially `codex-resources/bwrap`.
5. Enable `use_legacy_landlock = true`, rerun `pwd`, and capture the permission-profile panic.
6. Capture launcher `PATH` / resource lookup context if possible.

Linux-only source inspection can help find candidate code paths, but it is insufficient for PR evidence because the reported failure depends on Desktop-distributed WSL binary layout.

## 6-role synthesis

Fresh discovery after `2026-05-28T06:20:00Z` found `openai/codex#24873` as the only material runtime/tool-execution lead. `anthropics/claude-code#63016` is a Desktop UI shortcut-routing bug and lower fit; `#63017/#63018` are feature requests.

Existing runtime candidates keep priority for the general runner backlog: `probelabs/probe#568`, `google-gemini/gemini-cli#27503`, `vercel/ai#15652`, and `langchain4j/langchain4j#5313`. This Codex lane is higher strategic fit than docs-only leads, but it is WSL/Desktop-infra blocked.

## Critique

- Factology/duplicates: pass for `CANDIDATE`. No exact open PR cover found. `#21915` is lineage/prior art, not a blocker.
- Process gates: no upstream PR. A fact-finding `COMMENT-FIRST` comment could be useful later, but only after a clearer WSL evidence plan and without PR claims.
- Actionability: needs Windows + WSL2 + Codex Desktop repro. A Linux-only runner cannot close the main evidence gap.

## Decision

`next_status: CANDIDATE`

Keep as a Codex/Desktop WSL runtime candidate. Do not open upstream PR or claim root cause without Desktop WSL repro. Next action is runner/infra planning or a documented WSL fallback evidence path; otherwise watch for maintainer triage, labels, assignee, or linked PR.
