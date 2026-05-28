# Kilo / supermemory / Hindsight / GitNexus repo-scope watch, 2026-05-28 12:45 UTC

Контекст: очередной post-12:36 scouting pass по популярным AI-native / Paperclip-like репозиториям. Обязательные skills `open-source-bug-scouting` / `open-source-pr-workflow` в этой среде недоступны, поэтому использован documented fallback: 6 read-only scouting roles, parent live GitHub gates, duplicate/PR/process synthesis и tracker update. Upstream actions: `0`. Runner actions: `0`.

## 6-role synthesis

- Fresh discovery нашёл три незаписанных repo-scope surface: `supermemoryai/supermemory`, `vectorize-io/hindsight`, `abhigyanpatwari/GitNexus`; все популярные, свежие и тематически близкие к memory/context/code-intelligence слоям для agents.
- Active backlog refresh не нашёл новых material transitions для текущих runner candidates; `cmux#4944` уже merged, `cmux#4943`, `open-design#3210`, `apify#926`, `cognee#2914`, `OpenViking#2290/#2144` остаются watch/occupied.
- Duplicate/race роль подтвердила, что Apify/CloudBase/apm/vm0/MemOS lanes активны или заняты owner/automation PR queues; без runner/source pass их нельзя поднимать выше `WATCH`.
- Process gates: dedicated runner всё ещё не provisioned по `corp-opensource#10`; CT216 остаётся Hermes-only. Ни один lane не может перейти в `COMMENT-FIRST` или `PR-READY`.
- Actionability роль сохранила лучший no-runner source-first порядок: `claude-peers-mcp#64`, `Agent-S#195`, scoped `apify#917`, `vercel/ai#15652`, `gemini-cli#27503`. Fresh `openai/codex#24908` выглядит real-looking, но требует Linux GNOME Wayland repro и публичного patch-surface gate.
- Scope/materiality роль добавила `Kilo-Org/kilocode`: большой AI coding-agent platform repo; локально была только разовая ссылка на issue `#10149`, но repo-scope watch ещё не был записан.

## Parent live gates

### `Kilo-Org/kilocode`

- Public, non-fork, MIT, about 19.6k stars, pushed `2026-05-28T12:42:00Z`.
- Description/topics: agentic engineering platform / coding agent, VSCode/JetBrains/CLI, Claude/Gemini/Codex-adjacent topics.
- Fresh issues after cutoff were mostly assigned to maintainer `marius-kilocode` or process-bound migration/internal cleanup (`#10659/#10660/#10661`, plus assigned `#10655/#10654/#10639`).
- PR queue is active and partially blocked/draft: `#10658`, `#10656`, `#10644`, `#10638`, `#10637`, `#10631`, `#10627`, `#10626`, `#10624`, `#10620`, `#10617`, `#10614`, `#10612`, `#10611`.
- Decision: repo-scope `WATCH`, no issue-level candidate.

### `supermemoryai/supermemory`

- Public, non-fork, MIT, about 22.7k stars, pushed `2026-05-28T12:40:31Z`.
- Description/topics: Memory engine and Memory API for AI era; `agent-memory`, `ai-memory`, `memory`.
- Interesting open issues include `#985` privacy-safe retrieval receipts and `#792` Claude Code plugin writes memories but recall/search empty, but neither had enough source/repro/duplicate clearance for this cycle.
- PR queue is active; fresh `#1019` is blocked and many feature/fix PRs are in motion.
- Decision: repo-scope `WATCH`, no issue-level candidate.

### `vectorize-io/hindsight`

- Public, non-fork, MIT, about 15k stars, pushed `2026-05-28T12:37:55Z`.
- Description/topics: Agent memory that learns; `agentic-ai`, `memory`, `agents`, `ai-memory`.
- Interesting open issues include `#1801`, `#1796`, `#1786`, `#1758`, `#1751`, including Claude Code/OpenCode/OpenClaw integration signals.
- PR queue active with docs and feature/fix work (`#1816`, `#1803`, `#1799`, `#1798`, etc.).
- Decision: repo-scope `WATCH`, no issue-level candidate.

### `abhigyanpatwari/GitNexus`

- Public, non-fork, license metadata `Other`, about 40.6k stars, pushed `2026-05-28T12:38:52Z`.
- Description: zero-server code intelligence / knowledge graph / Graph RAG agent.
- Interesting open issues include `#1876` JavaScript duplicate Function/Const index bug and `#1871` hang/OOM on large C# codebase.
- PR queue is active and many lanes are blocked/dirty/unstable: `#1880`, `#1879`, `#1878`, `#1877`, `#1875`, etc.
- Decision: repo-scope `WATCH`, no issue-level candidate.

## Final decision

`next_status: WATCH`

Причина: material repo-scope discovery есть, но issue-level candidate не выбран. Все actionable-looking lanes требуют отдельного source checkout, duplicate refresh, contribution-process gate and runner-backed repro. Dedicated runner недоступен, а CT216 не является general open-source runner.

Следующий полезный bounded pass: отдельный source-first audit по `claude-peers-mcp#64` или `Agent-S#195`; repo-scope follow-up по Kilo/supermemory/Hindsight/GitNexus только если появится узкий unassigned bug с чистой PR/duplicate surface.
