# Agent of Empires post-12:08 watch, 2026-05-28 12:15 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Обязательные skills `open-source-bug-scouting` / `open-source-pr-workflow` в этом окружении всё ещё не установлены. Цикл выполнен через documented fallback: 6 read-only subagent ролей, parent live gates и internal critique.

Upstream comment, PR, ping, rerun или runner action не выполнялись.

`next_status: WATCH`

Fresh delta: `agent-of-empires/agent-of-empires` = repo-scope `WATCH`.

Upstream action count: `0`.

Runner action count: `0`.

## Parent live gates

Runner gate остаётся закрытым через [#10](https://github.com/serejaris/corp-opensource/issues/10): dedicated `corp-opensource-runner` не provisioned. CT `216` остаётся Hermes-only и не использовался для general repro.

| Lane | Live result | Decision |
|---|---|---|
| `agent-of-empires/agent-of-empires` | Новый repo-scope сигнал. На момент live gate: public, non-archived, MIT, `2,438` stars, pushed `2026-05-28 12:11:34 UTC`. Repo metadata: TUI/Web orchestration for Claude Code, OpenCode, Codex CLI, Gemini CLI, Hermes agent, Mistral Vibe, Pi.dev, Copilot CLI and Factory Droid Coding. Topics include `ai-coding`, `claude-code`, `opencode`, `terminal`, `tmux`, `codex`, `gemini-cli`, `orchestrator`, `hermes-agent`. Local README/watch search found no prior `agent-of-empires` entry. | Repo-scope `WATCH`; strong Paperclip-like/control-plane fit, but no selected bounded issue, source scan, repro, or PR-ready lane. |
| `agent-of-empires#1580/#1579` | Fresh cockpit/custom-agent feature lanes created `2026-05-28 10:23-10:32 UTC`, unassigned. | `WATCH`; feature/cockpit scope, not a bug candidate. |
| `agent-of-empires#1578` | Fresh Linux release binary/glibc compatibility issue, open and unassigned. | `WATCH`; packaging/release lane, needs release-matrix repro and process gate before any action. |
| `agent-of-empires#1568/#1563/#1562/#1559/#1525/#1517/#1512/#1511` | Multiple cockpit/web/TUI/session issues are already assigned to maintainers/contributors or P1 current-focus lanes. | `NO-GO for this cycle`; assignment/owner-focus gate and active PR queue make racing inappropriate. |
| Open PR queue | Several fresh owner/contributor PRs are open, many `BLOCKED`; examples include `#1577`, `#1576`, `#1575`, `#1574`, `#1573`, `#1572`, `#1571`, `#1560`, `#1549`. | `WATCH`; high churn and existing owner queue, no duplicate-safe patch surface selected. |

Other post-12:08 live signals did not produce a new actionable lane:

- `anthropics/claude-code#63111`: fresh has-repro Windows/VS Code permissions bug, but product/closed-source surface; `WATCH`.
- `vercel/ai#15670`: fresh perf issue, but open PR `#15669` appears to cover the reported O(N^2) accumulation path; `WATCH/duplicate-covered`.
- `nexu-io/open-design#3178/#3202`: claim/PR-covered after cutoff; `NO-GO`.
- `anomalyco/opencode#29733`: assigned to `Hona`; `NO-GO`.
- `openai/codex#12564/#24906`: product/feature or duplicate-clustered lanes; `WATCH`.

Carry-forward backlog stayed open but did not transition to `COMMENT-FIRST` or `PR-READY`: `probe#568`, `claude-peers-mcp#64`, `google-gemini/gemini-cli#27503`, `vercel/ai#15652/#15668`, `simular-ai/Agent-S#195`, `browseros-ai/BrowserOS#1005`, `generalaction/emdash#1875`, `topoteretes/cognee#2907`, `agentmemory#703-#708`, `OpenViking#2256`, `cmux`, `open-design`, and `clawd-on-desk`.

## 6-role synthesis

- Fresh discovery surfaced `vercel/ai#15668/#15664`, Gemini CLI EBADF/PTTY crash cluster, `mem0ai/mem0#5275/#5245`, `aaif-goose/goose#9434`, and low-star `openwong2kim/wmux#15`; none are PR-ready without repeat duplicate/process/source gates.
- Active backlog refresh found no material transition for core runner/source lanes. `vercel/ai#15668` remains duplicate-covered/risky because adjacent activeResponse work exists; `cmux`, `clawd-on-desk`, and `open-design` remain repo-scope watches.
- Duplicate/race guard found fresh post-cutoff occupied lanes: `open-design#3202 -> #3208`, `opencode#29733` assigned, `claude-code#63111` product-only, and `vercel/ai#15670 -> #15669`.
- Process gates confirmed no upstream action: runner unavailable, several repos/lane types are product, assignment, claim, or owner-queue gated.
- Actionability kept no-runner order as `claude-peers-mcp#64`, `Agent-S#195`, `gemini-cli#27503`, `vercel/ai#15652`, `vercel/ai#15668`, `cognee#2907`, `OpenViking#2256`; true runner order still starts with `probe#568`.
- Scope/materiality selected `agent-of-empires/agent-of-empires` as the only new repo-scope material delta after the 12:08 `clawd-on-desk` watch.

## Critique

- Factology/duplicates: record `agent-of-empires` only as repo-scope `WATCH`. Star count, license, push time and topics are live snapshots; no source checkout or full issue-by-issue duplicate proof was done. Active PR queue and assigned P1/P2 lanes make issue-level racing unsafe.
- Process gates: internal watch/tracker update is allowed; upstream comment/PR is blocked by no bounded selected bug, no runner-backed fail-before, no current-main repro, and no contribution/process pass for a concrete patch.
- Actionability: future pass should pick exactly one small terminal/session/control-plane bug, then repeat issue/PR search, source scan, contribution gate and runner/repro card before any status above `CANDIDATE`.

## Next

Keep `agent-of-empires/agent-of-empires` on repo-scope watch. Do not open an upstream comment or PR in this cycle.
