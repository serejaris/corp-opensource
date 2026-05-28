# Fresh AI-native watch, 2026-05-28 08:42 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this used the documented fallback: parent live GitHub gates, reused 6 read-only subagent roles, and focused duplicate/PR checks before status selection.

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Runner gate: dedicated `corp-opensource-runner` is still unavailable via [#10](https://github.com/serejaris/corp-opensource/issues/10). CT216 remains Hermes-only and was not used.

## Material deltas

- `openai/codex#24884`: the Windows Codex App WSL sandbox issue gained an external contributor intent comment at `2026-05-28T08:38:38Z`. The comment proposes bypassing the Windows sandbox only for the host-side `wsl.exe` bootstrap while relying on Linux-side sandboxing inside WSL. This increases duplicate/race risk and keeps the lane `WATCH`, not a local candidate.
- `anthropics/claude-code#63050`: fresh Monitor tool bug with `bug`, `has repro`, `platform:macos`, `area:tools`, `area:bash`. Monitor `command:` appears not to source the shell snapshot that Bash uses, so PATH-dependent tools such as `jq`, `sleep`, and `grep` fail on macOS/Nix. Exact PR search returned no cover, but this is product-owned and stays `WATCH`.
- `router-for-me/CLIProxyAPI#3594`: fresh Amp Neo CLI WebSocket support request. HTTP passthrough works, but Amp Neo's Rivet actor WebSocket path fails through CLIProxyAPI. Exact PR search returned no cover. Keep as `LEAD`: potentially relevant proxy/agent-tooling lane, but protocol/session complexity blocks candidate promotion.
- `google-gemini/gemini-cli#27517`: fresh `ioctl(2) failed, EBADF` crash in `@lydell/node-pty` resize under VS Code/xterm.js, Gemini CLI `0.44.0`, Linux, no sandbox. Exact search found only itself plus an old closed SIGHUP issue. Keep weak `WATCH` until repro is stronger.
- `anomalyco/opencode#29703`: project path/session migration feature request is compliance-gated and duplicate-flagged by bot against `#27822/#23249`, with assignee `kitlangton`; keep `NO-GO/WATCH`.

## Parent live gates

`codex#24884`:

- State: open, label `app`, no assignee.
- Fresh comment: external contributor `prashant2007-wq` asked to work on it and described a WSL-bootstrap-only sandbox exception.
- Decision: `WATCH / race-high`; no upstream comment or PR.

`claude-code#63050`:

- State: open, no assignee/comments at gate time.
- Created: `2026-05-28T08:37:35Z`; updated: `2026-05-28T08:38:35Z`.
- Labels: `bug`, `has repro`, `platform:macos`, `area:tools`, `area:bash`.
- Focused search for `Monitor shell snapshot PATH jq sleep Bash macOS Nix` found only `#63050`; focused PR search returned no cover.
- Decision: product `WATCH`; no external patch lane.

`CLIProxyAPI#3594`:

- State: open, no labels/assignees/comments at gate time.
- Created/updated: `2026-05-28T08:38:47Z`.
- Focused issue search for `Amp Neo WebSocket Rivet websocket upgrade thread execution` found only `#3594`; focused PR search returned no cover.
- Decision: `LEAD`; not candidate until protocol surface and current-main repro are clearer.

`gemini-cli#27517`:

- State: open, labels `status/need-triage`, `area/core`, no assignee/comments.
- Created: `2026-05-28T08:40:08Z`; updated: `2026-05-28T08:40:54Z`.
- Focused search for `ioctl node-pty resize xterm` found `#27517` plus old closed `#21073`, not an exact cover.
- Decision: weak `WATCH`; needs maintainer triage or reproducible trigger.

`claude-peers-mcp#64`:

- State: open, no labels/assignees/comments, unchanged since `2026-05-28T08:20:54Z`.
- Focused PR search for `max_tokens max_completion_tokens gpt-5.4-nano` returned no cover.
- Decision: remains the only current small source-level `CANDIDATE-needs-runner-validation`, not `PR-READY`.

## 6-role synthesis

- Fresh discovery found `claude-code#63050`, `CLIProxyAPI#3594`, `gemini-cli#27517`, and weak/no-go opencode compliance issues.
- Active refresh found only the post-08:36 `codex#24884` contributor-intent transition and no transition for `pydantic-ai#5670`, `vercel/ai#15665`, `claude-peers-mcp#64`, or `claude-code#63048`.
- Duplicate/race found no exact PR cover for `claude-code#63050`, `CLIProxyAPI#3594`, `gemini-cli#27517`, or `claude-peers-mcp#64`, but `codex#24884` now has high race risk and `opencode#29703` is duplicate/assignee gated.
- Process gate rejected upstream actions because the mandatory skills are unavailable, runner `#10` is closed to repro work, CT216 is Hermes-only, and no lane has parent gates plus runner evidence plus critique for `PR-READY`.
- Actionability ranked `claude-peers-mcp#64` as the best compact source-level candidate, followed by existing runner backlog `vercel/ai#15652`, `probelabs/probe#568`, and `gemini-cli#27503`.
- Synthesis selected cycle `next_status: WATCH` because fresh material is watch/lead only; candidate carry-forward does not change without runner validation.

## 3-role critique

- Factology/duplicates: approved after correcting product-owned `claude-code#63050` from possible candidate to watch-only in this repo's workflow; exact searches found no PR cover.
- Process gates: approved for internal watch/tracker update only. Upstream PR/comment count stays `0`.
- Actionability: no runner work; no upstream comment. Next real promotion remains runner-backed validation of `claude-peers-mcp#64` or existing runner backlog when `corp-opensource-runner` is provisioned.
