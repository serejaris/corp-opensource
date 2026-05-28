# App-builder / protocol-UI repo-scope watch, 2026-05-28 13:00 UTC

Контекст: bounded scouting pass после heartbeat `2026-05-28 12:53 UTC`. Обязательные skills `open-source-bug-scouting` / `open-source-pr-workflow` в этой среде недоступны, поэтому использован documented fallback: 6 read-only scouting roles, parent live GitHub gates, duplicate/PR/process synthesis и tracker update. Upstream actions: `0`. Runner actions: `0`.

## 6-role synthesis

- Fresh discovery не нашёл post-cutoff contribution-ready issue. Fresh Claude/Codex/opencode signals были product/private, duplicate-labelled, billing/rate-limit, assigned or low-signal. `openai/codex#24908` остаётся pre-cutoff technical watch only.
- Active backlog refresh не нашёл material lane transition после `12:53 UTC`; `pydantic-ai#5698` остаётся open/non-draft and blocked, поддерживая `pydantic-ai#5696` как duplicate-covered.
- Duplicate/race guard подтвердил: `pydantic-ai#5696`, `vercel/ai#15670`, `open-design#3202`, `agent-of-empires#1549/#1571`, `opencode#29734` are covered/occupied. Core candidates remain clean but runner/source-blocked.
- Process gates: `corp-opensource-runner` не provisioned по `#10`; CT221 только proposed, CT216 Hermes-only. Ни один lane не может перейти в `COMMENT-FIRST` или `PR-READY`.
- Actionability ranking сохранил order: no-runner `claude-peers-mcp#64`, `Agent-S#195`, scoped `apify#917`, `vercel/ai#15652`, `gemini-cli#27503`; with runner `probe#568` first.
- Scope/materiality нашёл missing app-builder/protocol surfaces: `dyad-sh/dyad`, `onlook-dev/onlook`, `MCP-UI-Org/mcp-ui`, `zgsm-ai/costrict`.

## Parent live gates

### `dyad-sh/dyad`

- Public, non-fork, not archived, about 20.4k stars, license metadata `Other`, pushed `2026-05-28T11:36:40Z`.
- Description: local open-source AI app builder / v0, Lovable, Replit and Bolt alternative.
- Open issue surface: about 237 issues. Recent examples include `#3530` plan mode accepted plan does not open implementation chat, `#3517` mode switch drops attached components, `#3513` Windows profile path spaces setup failure.
- Open PR queue: about 21 PRs, with active bot/owner E2E deflake and telemetry/provider validation work (`#3529`, `#3528`, `#3527`, etc.).
- Decision: repo-scope `WATCH / license-and-runner-gated`; no issue-level candidate yet.

### `onlook-dev/onlook`

- Public, non-fork, not archived, Apache-2.0, about 25.8k stars, updated `2026-05-28T10:30:02Z`.
- Description: AI-first visual design tool for visually building, styling and editing React apps.
- Open issue surface is older/noisy: examples include CodeSandbox Penpal timeout `#3087`, auth-optional unauthorized errors `#3051`, self-hosted publish socket close `#3030`.
- Open PR queue is active at repo scale, but no clean post-cutoff issue-level lane was selected.
- Decision: repo-scope `WATCH / app-builder-scope`.

### `MCP-UI-Org/mcp-ui`

- Public, non-fork, not archived, Apache-2.0, about 4.9k stars, updated `2026-05-28T11:10:16Z`.
- Description: UI over MCP / generative UI protocol and SDK.
- Interesting issues include `#195` AppRenderer host-context bridge first-mount `Not connected`, `#193` AppRenderer setup abort during iframe load, `#189` AppFrame sandbox timeout cleanup.
- Open PR `#201` overlaps host-context bridge, and several older transport/frame PRs remain open.
- Decision: repo-scope `WATCH / protocol-ui-scope`; no cold PR while bridge PR overlap is active.

### `zgsm-ai/costrict`

- Public, non-fork, not archived, Apache-2.0, about 4.1k stars, pushed `2026-05-28T05:29:07Z`.
- Description: strict AI coder for enterprises, including AI Agent, AI CodeReview and AI Completion.
- Issue surface is small: `#1249` terminal copy/paste is assigned, `#1167` MCP allow-list override is older/enhancement-labelled, `#1169` cross-Windows changes not detected.
- Open PRs include draft `#1248` remote agents and docs `#1242`.
- Decision: repo-scope `LEAD/WATCH / verify-first`; no issue-level candidate yet.

## Final decision

`next_status: WATCH`

Material delta is repo-scope only. No upstream comment/PR: fresh issues are not clean, app-builder lanes need source/repro/license/process gates, protocol-UI has active bridge overlap, and core candidate backlog remains runner-blocked.
