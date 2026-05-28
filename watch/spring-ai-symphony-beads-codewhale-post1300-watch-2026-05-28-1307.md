# Spring AI / Symphony / Beads / CodeWhale post-13:00 watch, 2026-05-28 13:07 UTC

Контекст: bounded scouting pass после heartbeat `2026-05-28 13:00 UTC`. Обязательные skills `open-source-bug-scouting` / `open-source-pr-workflow` в этой среде недоступны, поэтому использован documented fallback: 6 read-only scouting roles, parent live GitHub gates, duplicate/PR/process synthesis и tracker update. Upstream actions: `0`. Runner actions: `0`.

## 6-role synthesis

- Fresh discovery нашёл два issue-level сигнала: `spring-projects/spring-ai#6196` по MCP `resources/subscribe` in streamable sync WebMVC and `apify/actor-rag-web-browser#88` по error signaling for crawl/search failures.
- Active backlog refresh не нашёл material transition для core candidates; `gemini-cli#27528` попал в уже-known EBADF duplicate cluster, `herdr#334/#335` owner-handled or fixed-in-master, `Kilo/GitNexus/Apify/vm0` changes are repo-scope watch only.
- Duplicate/race guard подтвердил carry-forward clean candidates: `probe#568`, `claude-peers-mcp#64`, `gemini-cli#27503`, `vercel/ai#15652`, `Agent-S#195`, plus reserves. Но все остаются runner/source gated.
- Process gates: `corp-opensource-runner` не provisioned по `#10`; CT221 только proposed, CT216 Hermes-only. Ни один lane не может перейти в `COMMENT-FIRST` или `PR-READY`.
- Actionability ranking сохранил same order: no-runner `claude-peers-mcp#64`, `Agent-S#195`, scoped `apify#917`, `vercel/ai#15652`, `gemini-cli#27503`; with runner `probe#568` first.
- Scope/materiality нашёл missing repo-scope surfaces: `openai/symphony`, `gastownhall/beads`, `Hmbown/CodeWhale`, `mksglu/context-mode`, `get-convex/chef`.

## Parent live gates

### `spring-projects/spring-ai#6196`

- Issue: [`#6196`](https://github.com/spring-projects/spring-ai/issues/6196), open, created `2026-05-28T13:02:15Z`, label `status: waiting-for-triage`, no assignee.
- Repo: public, non-fork, Apache-2.0, about 8.8k stars, pushed `2026-05-28T12:21:19Z`.
- Report: MCP server exposes resource templates but streamable sync WebMVC reports `resources.subscribe: false`; `notifyResourcesUpdated` becomes useless without `resources/subscribe`.
- Parent open PR sample did not show an obvious exact cover; visible recent PRs are DeepSeek constants, telemetry span attributes, Neo4j chat memory, MCP annotations draft, etc.
- Decision: internal `CANDIDATE-needs-repro/process-gate`, not upstream-ready. Needs source checkout, exact duplicate search, current-main repro, and contribution gate before any comment/PR.

### `apify/actor-rag-web-browser#88`

- Issue: [`#88`](https://github.com/apify/actor-rag-web-browser/issues/88), open, created `2026-05-28T13:01:05Z`, no assignee.
- Repo: public, non-fork, Apache-2.0, about 72 stars, pushed `2026-05-01T20:46:17Z`.
- Report: `/search` returns `200` and empty content when crawl/fetch fails; asks for non-2xx status or explicit error field so clients can distinguish no results from failure.
- Open PR queue has only old `#61` performance evaluation; no visible exact cover.
- Decision: `WATCH / CANDIDATE-backup`. Actionable API semantics, but small repo and behavior may be intentional/backcompat-sensitive; needs maintainer preference or repro before comment/PR.

### `openai/symphony`

- Public, non-fork, Apache-2.0, about 24.8k stars, issues disabled, updated `2026-05-28T12:41:26Z`.
- Description: isolated autonomous implementation runs; direct Paperclip-like control-plane / autonomous-run manager.
- Open PR queue has only three older PRs: `#65`, `#60`, `#58`.
- Decision: passive repo-scope `WATCH`; no issue lane because issues are disabled.

### `gastownhall/beads`

- Public, non-fork, MIT, about 24.2k stars, pushed `2026-05-28T10:51:27Z`.
- Description: memory upgrade for coding agents.
- Open issues include `#4214` GitHub sync dirty state never clears, `#4210` Codex-only hooks causing Claude Code errors, `#4208` false prefix mismatch, `#4164` custom status claim handling.
- PR queue is large at about 178 PRs, so future lanes need careful duplicate/race gating.
- Decision: repo-scope `WATCH`; no issue-level candidate selected this cycle.

### `Hmbown/CodeWhale`

- Public, non-fork, MIT, about 35.5k stars, pushed `2026-05-28T05:07:36Z`.
- Description: DeepSeek + MiMo coding agent in terminal.
- Fresh issues include `#2317` overly long reply blocks follow-up, `#2310` cannot start message with `/`, `#2303` `allow_shell` default blocks one path but not another.
- PR queue is high velocity, and nearby slash/composer work has overlap risk.
- Decision: repo-scope `WATCH / verify-first`; no cold PR.

### `mksglu/context-mode`

- Public, non-fork, license metadata `Other`, about 15.9k stars, pushed `2026-05-27T13:03:31Z`.
- Description: context-window optimization for AI coding agents, sandboxing tool output and supporting multiple platforms.
- Decision: repo-scope `WATCH / license-gated reference`.

### `get-convex/chef`

- Public, non-fork, Apache-2.0, about 4.6k stars, pushed `2026-04-22T01:33:38Z`.
- Description: AI app builder that knows backend.
- Lower current contribution payoff than `dyad`/`onlook`; issues are limited and mostly support/noisy.
- Decision: repo-scope `WATCH / low-priority app-builder`.

## Final decision

`next_status: WATCH`

Material delta exists both at issue-level and repo-scope, but no upstream action is allowed. `spring-ai#6196` is the strongest fresh internal candidate, while `apify/actor-rag-web-browser#88` is a backup candidate. Both require source/repro/duplicate/process gates. Repo additions are `WATCH` only.
