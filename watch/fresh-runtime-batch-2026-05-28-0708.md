# Fresh runtime batch 2026-05-28 07:08 UTC

Дата: 2026-05-28

Trackers:

- Umbrella: https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561586615
- Follow-up: https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561756753

Статус: `CANDIDATE`

Upstream actions: `0`

Runner actions: `0`

## Коротко

Fresh discovery after `2026-05-28T06:53:00Z` found four material issues. Parent decision: select `NousResearch/hermes-agent#33712` as the only fresh candidate from this batch; record the other three as watch / covered-risk.

## Selected candidate

`NousResearch/hermes-agent#33712`: https://github.com/NousResearch/hermes-agent/issues/33712

- Live gate: open, unassigned, no labels, no comments at gate time.
- Symptom: `hermes dashboard` fails on NixOS/container with `ModuleNotFoundError: No module named 'hermes_cli.dashboard_auth'`.
- Duplicate/PR search: no exact cover found for `dashboard_auth` missing-module packaging failure.
- Adjacent PR: `#33311` touches `dashboard_auth`, but it fixes reverse-proxy login URL prefixes and does not cover a missing packaged module.
- Repro blocker: needs NixOS/container or Hermes-specific package smoke. CT `216` is Hermes-only and not a general runner; no runner action was taken in this cycle.

`next_status: CANDIDATE`

## Watch / covered-risk lanes

`openai/codex#24879`: https://github.com/openai/codex/issues/24879

- Auto-review sends hardcoded `codex-auto-review` model to custom provider.
- Material Codex/Paperclip-fit bug, but duplicate/cover risk is high.
- Same-title `#24877` was opened and closed minutes earlier by the same author.
- Existing `#21928` covers similar `codex-auto-review` custom provider incompatibility.
- Active PR `#23767` directly changes auto-review model override / selection behavior.
- Decision: `WATCH / covered-risk`, not candidate until `#23767` lands and current-main mock-provider repro still fails.

`anthropics/claude-code#63025`: https://github.com/anthropics/claude-code/issues/63025

- SSH Remote / Desktop restart data-index bug: remote `~/.claude.json` loses `projects`, while JSONL transcripts remain intact.
- Severity is high because the UI shows "No messages yet" for existing sessions, but repro requires Windows Desktop + SSH Remote and the issue already maps related history-loss issues.
- Decision: `WATCH / COMMENT-FIRST only with new evidence`; no upstream comment in this cycle.

`NVIDIA/TensorRT-LLM#14676`: https://github.com/NVIDIA/TensorRT-LLM/issues/14676

- AutoDeploy model-coverage failures with `RuntimeError: NVRTC compilation failed` across multiple models.
- Open, labeled `bug`, `Customized kernels`, `AutoDeploy`; no exact duplicate found.
- Repro requires GPU/AutoDeploy CI/NVRTC context and is likely maintainer-internal.
- Decision: `LEAD/WATCH`, not a current contribution target.

## 6-role synthesis

Factology/duplicate critique selected Hermes `#33712` as the cleanest candidate: exact duplicate search is clean, adjacent `#33311` is not a cover, and the symptom has a plausible package/module inclusion surface. Process critique blocks upstream PR/comment without NixOS/container repro and package-surface confirmation. Actionability critique notes that Codex `#24879` has the most compact mock-provider repro, but active PR `#23767` makes it covered-risk for now.

Existing runner order remains unchanged: Probe, Gemini, Vercel `#15652`, LangChain4j, then Nanobot/Hermes fallback candidates. No upstream comments or PRs were made.

## Decision

`next_status: CANDIDATE`

Track Hermes `#33712` as a fresh packaging/runtime candidate. Keep Codex `#24879`, Claude `#63025`, and TensorRT-LLM `#14676` in watch lanes.

## Follow-up, 2026-05-28 07:34 UTC

Live delta after the original gate:

- `NousResearch/hermes-agent#33712` is still open, unassigned, and has no comments, but upstream added labels `type/bug`, `comp/cli`, `area/nix`, and `P3` at `2026-05-28T07:24:35Z`.
- This confirms upstream triaged the lane as a CLI/Nix bug, while `P3` keeps it a lower-urgency candidate.
- Exact PR cover still was not found; adjacent `#33311` remains a dashboard-auth reverse-proxy login URL PR, not a missing `hermes_cli.dashboard_auth` package-module fix.

Claude cluster delta:

- `anthropics/claude-code#63028` is a fresh Web/plugin lead: declared plugins are inactive during the first Claude Code Web cloud session, with labels `bug`, `has repro`, `area:hooks`, `area:claude-code-web`, `platform:web`, and `area:plugins`. No exact PR cover found. Keep as `WATCH / comment-first only with new evidence`; not a local PR lane.
- `anthropics/claude-code#63025` received a detailed third-party comment showing the same "No messages yet" UI symptom while the `projects` map remains intact. This weakens the original null-`projects` root-cause framing and broadens the likely SSH Remote rehydration surface. Keep as `WATCH / diagnostic thread`.
- `anthropics/claude-code#63029` is a weaker Web archive-restore data-loss report; duplicate/process risk is high, so keep only as passive watch.

Decision remains: `next_status: CANDIDATE` for Hermes `#33712`, upstream actions `0`, runner actions `0`. No upstream comment/PR without Nix/container package-surface repro. Runner `#10` remains unavailable.
