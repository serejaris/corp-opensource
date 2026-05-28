# Claude / Codex / opencode post-10:26 watch, 2026-05-28 10:32 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52), comment [#4563197735](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4563197735)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are still not installed in this environment, so this cycle used the documented fallback: parent live GitHub gates, 6 read-only subagent roles, duplicate/PR searches, and process/actionability/duplicate critique.

No upstream comment, PR, ping, rerun, or runner action was made.

`next_status: CANDIDATE`

Upstream action count: `0`.

Runner action count: `0`.

## Parent live gates

Runner gate remains closed via [#10](https://github.com/serejaris/corp-opensource/issues/10): no dedicated `corp-opensource-runner` is provisioned. CT `216` remains Hermes-only and was not used.

| Lane | Live result | Decision |
|---|---|---|
| `openai/codex#24890` | Closed at `2026-05-28 10:29:01 UTC` after the author said Codex already supports the `resource` parameter through `codex mcp add ... --oauth-resource ...`; their case was missing config. Open cluster `#13891` remains for the broader OAuth resource-indicator/token-exchange gap with fork-fix evidence. | `WATCH / closed-no-action` for `#24890`; keep `#13891` as duplicate-heavy product/process watch, not our PR lane. |
| `anthropics/claude-code#63081` | Open Cowork plugin upload bug with minimal repro for `SKILL.md` frontmatter `description` containing `<...>`. Duplicate bot linked `#61283/#56376/#16754`; `#61283` is a closed duplicate and `#56376` is an open stale generic validation-error UX issue. | `WATCH / duplicate-risk product lane`; no runner or upstream comment. |
| `anthropics/claude-code#63082` | Fresh Desktop App data-loss/session-persistence regression with `has repro`: startup scanner removes `cliSessionId` and sets `transcriptUnavailable: true`; CLI resume still works. Search found related Desktop/session-loss cluster such as `#48334/#53717`, but no exact PR cover. | `WATCH / product-owned Desktop lane`; useful signal, but no public OSS patch path or runner harness. |
| `anthropics/claude-code#63074` | Fresh author comment adds OTel `session.id` missing on `claude_code.interaction` span, not only log events. Search found no exact PR, but `#55269` remains strong open adjacent OTel correlation issue. | `WATCH / duplicate-adjacent`; no comment/PR without maintainer ask. |
| `anomalyco/opencode#29721/#29722` | `/goal` issue gained exact linked PR `#29722`, plus compliance and duplicate signals. Existing `#27167/#29445` and PRs `#28610/#27163` cover the same goal-mode feature family; author noted `#27163` is more complete. | `NO-GO / covered + duplicate-risk + compliance-gated`. |

Carry-forward runner candidates had no transition after 10:26: `probe#568`, `claude-peers-mcp#64`, `vercel/ai#15652`, and `gemini-cli#27503` remain `CANDIDATE-needs-runner-validation`. `opencode#26187` remains a conditional runner/source-test candidate but is assignment-gated and not `PR-READY`.

## 6-role synthesis

- Fresh discovery surfaced Claude `#63081/#63082` and opencode `#29721/#29722`. Claude lanes are product/Desktop/Cowork watch only; opencode `/goal` is exact-covered and duplicate-heavy.
- Active-lane refresh found no post-10:26 transition for core runner candidates; `codex#24890` is the only material closed-no-action correction.
- Duplicate/race critique kept Claude plugin/session/OTel lanes out of implementation, marked opencode `/goal` no-go, and preserved Gemini `#27503` plus Vercel `#15652` as real runner candidates.
- Process critique allowed internal tracker/watch update only; upstream comment/PR and general runner actions remain blocked.
- Actionability critique keeps runner order: `probe#568`, `claude-peers-mcp#64`, `Agent-S#195`, then Vercel/Gemini depending on runner tooling.

## 3-role critique

- Factology/duplicates: `codex#24890` must now be described as author-closed config/user-side, while `codex#13891` remains the broader open OAuth resource lane. Do not claim `#24890` is still an active duplicate report.
- Process gates: no upstream comment, PR, rerun, or runner action is allowed.
- Actionability: final backlog status remains `CANDIDATE`, but no fresh lane is `COMMENT-FIRST` or `PR-READY`.

## Next

Keep `probe#568` first for the future dedicated runner. Recheck `codex#13891` only for an official PR/maintainer decision. Treat Claude `#63081/#63082/#63074` as product-watch signals unless Anthropic asks for external evidence or exposes a public patch path.
