# AionUi / AionCore #3051 Unicode ACP text loss, 2026-05-28

Scope: fresh hot local-agent / browser-agent UI slice covering `iOfficeAI/AionUi`, `iOfficeAI/AionCore`, `jackwener/OpenCLI`, `zhayujie/CowAgent`, `Panniantong/Agent-Reach`, and `browser-use/web-ui`.

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills were not available in this environment, so this used the documented fallback: parent live GitHub gates, source search, 6 read-only roles, then 3-role critique before recording status.

Upstream actions: `0`. Runner actions: `0`.

Tracker comment: umbrella [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560930367).

Final `next_status`: `CANDIDATE-needs-runner-repro` for [`iOfficeAI/AionUi#3051`](https://github.com/iOfficeAI/AionUi/issues/3051), implemented/fixable only if the failing path is confirmed in public [`iOfficeAI/AionCore`](https://github.com/iOfficeAI/AionCore) source.

## Parent Live Gates

Time window: `2026-05-28 04:48-05:02 UTC`.

| Repo / lane | Live state | Decision |
| --- | --- | --- |
| [`iOfficeAI/AionUi#3051`](https://github.com/iOfficeAI/AionUi/issues/3051) | Open `bug`, no comments/assignee. Report says OpenCode via ACP drops non-ASCII text responses (`əıöüşçğ` etc.): SQLite stores `thinking` rows but no `type=text` row, and logs show `StreamRelay received terminal event` with `text_len=0`. Exact PR search for `#3051`, OpenCode ACP non-ASCII text, `aioncore`, and `StreamRelay` found no cover. | `CANDIDATE-needs-runner-repro`. Source path exists, but the exact failing conversion layer is not proven. |
| [`iOfficeAI/AionUi#3046`](https://github.com/iOfficeAI/AionUi/issues/3046) / [`#3047`](https://github.com/iOfficeAI/AionUi/issues/3047) | Open reports that OpenCode native MCP servers and Oh-my-openagent plugins are unavailable through ACP. Open PR [`#3049`](https://github.com/iOfficeAI/AionUi/pull/3049) says `Fixes #3047` / `Closes #3048` and describes native MCP merge plus plugin passthrough. | `WATCH / duplicate-covered`; no competing PR/comment while `#3049` is live. |
| [`jackwener/OpenCLI#1760`](https://github.com/jackwener/OpenCLI/issues/1760) | Open tab-group residue bug where automation can reuse user tabs. Open PR [`#1762`](https://github.com/jackwener/OpenCLI/pull/1762) is mergeable, green, and changes `extension/src/background.ts` plus tests with exact title coverage. | `WATCH / duplicate-covered`. |
| [`iOfficeAI/AionUi#3044`](https://github.com/iOfficeAI/AionUi/issues/3044) | Open Codex CLI message-order/merge report with screenshot and stable repro claim, but no source-localized path yet. | `LEAD / WATCH`; weaker than `#3051` until source localization and text fixture exist. |

## Public Source Gate

Initial parent search in `AionUi` only found `aioncore` packaging/downloader references, which made `#3051` look like a compiled-binary blocker. The 6-role cycle raised a possible sibling source repo, then parent verified it:

- `iOfficeAI/AionCore` is public, Apache-2.0, latest release [`v0.1.14`](https://github.com/iOfficeAI/AionCore/releases/tag/v0.1.14), pushed `2026-05-28`.
- `AionUi` downloads release assets from `iOfficeAI/AionCore` in `packages/shared-scripts/src/prepare-aioncore.js` and `scripts/prepareAioncore.js`.
- `AionCore` contains `crates/aionui-conversation/src/stream_relay.rs`, including:
  - `AgentStreamEvent::Text(data)`;
  - `segment.buffer.push_str(&data.content)`;
  - `full_text_buffer.push_str(&data.content)`;
  - terminal log field `text_len = full_text_buffer.len()` with message `StreamRelay received terminal event`.
- Code search also found ACP/session-flow surface in `crates/aionui-ai-agent/src/manager/acp/agent_session_flow.rs` and related integration tests.

This proves a public source surface exists. It does not prove the bug is in `StreamRelay` itself; the loss may be lower in the ACP/OpenCode conversion path.

## Six-role Synthesis

- Factology/source: `AionUi#3051` is the only fresh high-fit lane not already duplicate-covered. Initial source gate was incomplete until parent verified public `AionCore`.
- Duplicate/race: `AionUi#3046/#3047` and `OpenCLI#1760` are covered by live PRs. Do not race them.
- Process: no upstream action. `#3051` needs runner evidence and a precise public-source failing test before `COMMENT-FIRST` or PR.
- Actionability/source: test direct `AgentStreamEvent::Text` Unicode persistence first; if that passes, move down to the ACP/OpenCode conversion layer.
- Docs/tracker: record a short watch note and umbrella synthesis because the public-source discovery is material.
- Challenger: do not create a dedicated issue or README entry until runner repro proves a failing current-main path.

## Three-role Critique

- Factology/duplicates: `#3051` can be internal `CANDIDATE-needs-runner-repro`; exact PR search was clean and public patch surface is now verified.
- Process: PR/comment is forbidden without runner repro/test and maintainer-useful evidence. Upstream action count stays `0`.
- Actionability: create this note and short umbrella `#52` comment. Skip README and dedicated tracker issue.

## Runner / Repro Plan

Runner required: yes. This is Rust backend + SQLite/async stream relay behavior, and repo policy requires stable runner evidence before upstream action.

1. In `corp-opensource-runner`, clone `iOfficeAI/AionCore` at current main and record the exact release linkage from `AionUi`.
2. Add or run a focused unit/integration test that sends `AgentStreamEvent::Text` with `content = "əıöüşçğ"` through `StreamRelay::consume`.
3. Assert persistence includes a `type = "text"` row and JSON content with the Unicode text, not only a `thinking` row.
4. If direct `TextEventData` passes, build a lower-level synthetic ACP/OpenCode terminal event test and verify it becomes `AgentStreamEvent::Text`.
5. Only if a fail-before is captured and the patch surface remains minimal should the lane move toward `PR-READY`.

## Re-entry Triggers

- `AionUi#3051` receives maintainer source guidance, assignment, or a linked PR.
- An exact cover PR appears, closes without merge, or lands without covering non-ASCII text persistence.
- Runner becomes available and can capture a current-main failing regression.
- Another issue confirms the same OpenCode ACP Unicode text-loss scope.
- `AionUi#3046/#3047` re-enter only if `#3049` closes unmerged or lands incomplete; `OpenCLI#1760` re-enters only if `#1762` closes unmerged or leaves a gap.
