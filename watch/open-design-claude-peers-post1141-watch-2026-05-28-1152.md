# Open Design / claude-peers post-11:41 watch, 2026-05-28 11:52 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Обязательные skills `open-source-bug-scouting` / `open-source-pr-workflow` в этом окружении всё ещё не установлены. Цикл выполнен через documented fallback: 6 read-only subagent ролей, parent live gates и focused 3-role critique.

Upstream comment, PR, ping, rerun или runner action не выполнялись.

`next_status: CANDIDATE`

Fresh delta: repo-scope cluster = `WATCH`; `claude-peers-mcp#64` = source-first `CANDIDATE/fallback`, not `PR-READY`.

Upstream action count: `0`.

Runner action count: `0`.

## Parent live gates

Runner gate остаётся закрытым через [#10](https://github.com/serejaris/corp-opensource/issues/10): dedicated `corp-opensource-runner` не provisioned. CT `216` остаётся Hermes-only и не использовался для general repro.

| Lane | Live result | Decision |
|---|---|---|
| `nexu-io/open-design` | Новый repo-scope сигнал. На момент live gate: public, non-archived TypeScript repo; Apache-2.0; `54,480` stars, `6,172` forks, pushed `2026-05-28 11:47:12 UTC`. Description covers local-first Claude Design alternative and runs on Claude Code, Codex, Cursor, Gemini, OpenCode, Qwen, Copilot, Hermes, Kimi CLI. Topics include `ai-agents`, `coding-agents`, `ai-design`, `local-first`, `claude-code-for-design`, `codex-design`, `hermes-agent`, `agent-skills`, `desktop-app`. Local README/watch search found no prior `open-design` entry. | Repo-scope `WATCH`; strong Paperclip-like design-agent surface, but no issue-level lane, source scan, repro, or PR-ready duplicate gate yet. |
| `nexu-io/open-design` issue/PR queue | Open issues are high-churn UI/design-agent bugs such as `#3202`, `#3203`, `#3206`; open PRs include fresh review-gated work such as `#3207`, `#3204`, `#3198`, and many older active PRs. | `WATCH`; choose one bounded bug in a future cycle before any candidate promotion. |
| `wanshuiyin/Auto-claude-code-research-in-sleep` | Repo-scope signal. Public Python repo; MIT; `10,903` stars, `1,033` forks, pushed `2026-05-28 11:47:49 UTC`; autonomous ML research / markdown-only skills for Claude Code, Codex, OpenClaw and other LLM agents. No new clean bug lane found in latest issue/PR list. | Passive repo-scope `WATCH`; not candidate. |
| `getpaseo/paseo` | Repo-scope signal. Public TypeScript repo; license `Other`; `6,846` stars, `651` forks, pushed `2026-05-28 11:41:25 UTC`; coding agents from phone, desktop and CLI. Open PR `#1195` adds model gateway support and is blocked; fresh issues include desktop/UI and daemon pairing lanes. | Repo-scope `WATCH`; license/process and active PR churn block cold action. |
| `SeemSeam/claude_codex_bridge` | Repo-scope signal. Public Python repo; license `Other`; `2,800` stars, `278` forks, pushed `2026-05-28 11:45:41 UTC`; visible multi-agent CLI teams for Claude, Codex, Gemini, OpenCode and Droid with project memory/tmux supervision. | Repo-scope `WATCH`; existing issues are environment-heavy and PR queue has older unresolved bridge work. |
| `OpenBMB/PilotDeck` | Repo-scope signal. Public TypeScript repo; AGPL-3.0; `1,221` stars, `85` forks, pushed `2026-05-28 11:45:19 UTC`; task-oriented AI Agent productivity platform. Fresh issues include `#38` skills install error and Windows/setup problems; open PRs are already active around Windows bootstrap/security/deps. | Lower-priority repo-scope `WATCH`; small/new repo, AGPL, noisy startup issue queue. |
| `louislva/claude-peers-mcp#64` | Open, unassigned, no labels/comments. Exact open PR searches for `max_completion_tokens` and `max_tokens` returned empty. Source check found `shared/summarize.ts` sends OpenAI chat completions with `model: "gpt-5.4-nano"` and `max_tokens: 100`; this matches the reported API-contract risk. | Source-first `CANDIDATE/fallback`; not runner-backed fail-before, not `COMMENT-FIRST`, not `PR-READY`. Repeat duplicate/process gate before any upstream action. |
| `openai/codex#24903` | Fresh open non-draft PR created `2026-05-28 11:42:11 UTC`: `Reap stale multi-agent slots`, touching agent control and multi-agent close handlers/tests. | `WATCH`; occupied upstream PR lane, useful only as Codex multi-agent lifecycle signal. |
| `openai/codex#24904` | Fresh open bug created `2026-05-28 11:47:29 UTC`: Windows Codex Desktop sidebar flicker / bad light-mode rendering with GPU acceleration. Exact PR search for sidebar/GPU/flicker returned empty, but related Windows/GPU/UI cluster is not clean. | `WATCH/CANDIDATE only with Windows/GPU repro`; no public action now. |
| `anthropics/claude-code#63103` | Fresh open web/integration bug created `2026-05-28 11:44:52 UTC`: Code review Preview "Add a repository" does not find private GitHub org repos. | Product/support `WATCH`; requires private org/admin/OAuth surface, no source-local candidate. |
| `anomalyco/opencode#29730` | Fresh open issue created `2026-05-28 11:39:59 UTC`, updated after cutoff, assigned to `nexxeln`; duplicate bot linked memory/OOM cluster. | `WATCH/NO-GO`; assigned and duplicate-clustered. |

Carry-forward source/runner lanes remain open but did not transition: `probe#568`, `google-gemini/gemini-cli#27503`, `vercel/ai#15652`, `simular-ai/Agent-S#195`, `browseros-ai/BrowserOS#1005`, `generalaction/emdash#1875`, `topoteretes/cognee#2907`, `agentmemory#703-#708`, `OpenViking#2256`, and `langchain#37736/#37737`.

## 6-role synthesis

- Fresh discovery found Codex/Claude product watch items and `openai/codex#24903` as an occupied upstream PR; none is upstream-actionable from this repo.
- Active refresh found no post-cutoff transition for the existing candidate backlog. Exact open PR search stayed empty for `probe#568`, `claude-peers-mcp#64`, `gemini-cli#27503`, `vercel/ai#15652`, and `Agent-S#195`.
- Duplicate/race guard found no safe promotion. `OpenViking#2288` is fresh but unrelated to `OpenViking#2256`; `cognee#2907` has old ontology PR references and still needs source/repro.
- Process gates keep upstream action blocked everywhere: runner unavailable, some lanes are assignment/author-owned, and candidate lanes need fail-before evidence.
- Actionability split: `probe#568` remains the first full runner target, while without runner the cheapest source-first fallback is now `claude-peers-mcp#64`, then `Agent-S#195`, then partial `gemini-cli#27503` source validation.
- Scope guard recommended recording a short repo-scope delta. Parent live gates narrowed it to a high-confidence `open-design` watch plus lower-priority ARIS/Paseo/bridge/PilotDeck watch signals.

## 3-role critique

- Factology/duplicates: `open-design` metadata is a live-gate snapshot and must remain repo-scope `WATCH`; no issue-level duplicate/race pass or source scan was done. `claude-peers-mcp#64` source evidence is real, but it is not runner-backed fail-before. `openai/codex#24904` has nearby Windows/GPU/UI duplicate cluster risk; `claude-code#63103` is private org/admin/OAuth surface; `opencode#29730` is assigned.
- Process gates: fallback scouting is recorded as a process risk, not a full substitute for the missing mandatory skills. Internal watch/tracker update is allowed; upstream comment/PR is blocked by missing runner evidence and missing fresh full gate immediately before action.
- Actionability: record repo-scope cluster separately from bug promotion. Overall `next_status: CANDIDATE` is only for the carry-forward backlog; fresh repo-scope cluster is `WATCH`, and `claude-peers-mcp#64` is source-first `CANDIDATE/fallback`, not `PR-READY`.

## Next

Keep `nexu-io/open-design` on repo-scope watch and do a future focused pass to pick exactly one bounded issue before promoting anything. For no-runner progress, `claude-peers-mcp#64` is the cheapest next source-first validation target; a future pass should build a mock-fetch/request-body fixture around `shared/summarize.ts` and repeat duplicate/process gates before any upstream comment or PR.
