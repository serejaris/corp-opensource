# Claude/opencode post-cutoff watch 2026-05-28 08:20

Дата: 2026-05-28

Tracker:

- Umbrella: https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562042732

Статус: `WATCH`

Upstream actions: `0`

Runner actions: `0`

## Коротко

Bounded refresh после `ca8723c` / tracker heartbeat `2026-05-28T08:13:23Z` нашёл несколько свежих signals, но ни один не стал clean implementation lane:

- `anthropics/claude-code#63043`: Windows Grep tool leaks `rg.exe` + `conhost.exe` processes across long sessions.
- `anthropics/claude-code#63044`: stats dashboard "Peak hour" appears off by one hour.
- `anomalyco/opencode#29699`: upstream PR keeps OpenAI websocket response timeouts active.
- `anomalyco/opencode#29692`: compliance cleared after reporter added Desktop repro for `erp1`/`erp2` folder confusion.

## Parent live gates

`anthropics/claude-code#63043`: https://github.com/anthropics/claude-code/issues/63043

- State: open.
- Created: `2026-05-28T08:12:17Z`; updated: `2026-05-28T08:13:25Z`.
- Labels: `bug`, `has repro`, `platform:windows`, `area:tools`, `perf:memory`.
- Assignee: none.
- Comments: `0`.
- Reported shape: Windows long sessions accumulate `rg.exe` and paired `conhost.exe`; reporter measured `1062` `rg.exe` plus `1088` `conhost.exe`, about `14 GB` total RAM.

`anthropics/claude-code#63044`: https://github.com/anthropics/claude-code/issues/63044

- State: open.
- Created: `2026-05-28T08:14:12Z`; updated: `2026-05-28T08:15:11Z`.
- Labels: `bug`, `has repro`, `platform:windows`, `area:tui`.
- Assignee: none.
- Comments: `0`.
- Reported shape: local JSONL aggregation shows 18:00 as the peak hour, while dashboard reports 19:00; likely timezone/hour-bucketing issue.

`anomalyco/opencode#29699`: https://github.com/anomalyco/opencode/pull/29699

- State: open, non-draft.
- Author: `Hona`.
- Mergeability: `MERGEABLE`.
- Review decision: none.
- Updated: `2026-05-28T08:14:48Z`.
- Scope: keep active Responses WebSocket stream idle timer refed and keep retry/fallback behavior from `#29673` unchanged.

`anomalyco/opencode#29692`: https://github.com/anomalyco/opencode/issues/29692

- State: open.
- Assignee: `Hona`.
- Updated: `2026-05-28T08:13:28Z`.
- Material delta: compliance bot accepted the added reproduction steps and environment.
- Reported shape: Desktop App opens sibling folder `erp1` when the user selects `erp2` on Windows 11, opencode `v1.15.11`.

## Duplicate / race

`Claude#63043`:

- Focused search for `Windows rg conhost Grep leak` and `Grep rg.exe conhost.exe zombies` found `#63043` as the exact issue.
- Adjacent closed `#33947` covers macOS MCP/subagent orphan accumulation, not Windows `rg.exe` + `conhost.exe` accumulation.
- PR search for `rg conhost Grep Windows Job Object` found no exact PR cover.
- Status impact: high-signal product-owned watch; no OSS patch lane.

`Claude#63044`:

- Focused search for `Peak hour timezone dashboard` found only `#63044` as an exact current issue.
- Product analytics/TUI surface makes external action low-value without maintainer ask.

`opencode#29699`:

- PR search for `websocket response timeout` shows live upstream PR `#29699`, adjacent open PR `#29645`, and merged transport PR `#29477`.
- Issue search shows related `#29646` about OpenAI websocket disconnect/retry. `#29699` is already the targeted upstream-owned implementation lane.

`opencode#29692`:

- `#29692` is now compliance-clear but assigned to `Hona`.
- Fresh nearby `#29698` is a weak same-folder-name Desktop project identity report; duplicate/race critique flags likely overlap with older path/name identity issues.
- No cold PR/comment; wait for assignee/maintainer signal.

## 6-role synthesis

Новые subagent threads недоступны из-за thread limit, поэтому цикл переиспользовал шесть уже перечисленных agents.

- Fresh discovery ranked `Claude#63043` as the strongest new signal after `08:13Z`, with `Claude#63044`, `opencode#29699`, `Claude#61993`, and `pydantic-ai#5230` as watch-only deltas.
- Active refresh found no post-cutoff close/merge/review/maintainer-request transition in existing tracked lanes.
- Duplicate/race critique found no exact PR cover for `Claude#63043/#63044`, but product-owned surface blocks contribution; `opencode#29699` is already an upstream PR and `opencode#29692` is assigned.
- Process critique blocks upstream comment/PR: required skills are unavailable, runner `#10` is unavailable, and new material items are product-owned, assigned, or already PR-covered.
- Actionability critique keeps `vercel/ai#15652`, `probelabs/probe#568`, and `google-gemini/gemini-cli#27503` as the top runner-backed backlog; no `PR-READY` lane exists while runner is unavailable.

## Decision

`next_status: WATCH`

Track `Claude#63043` as high-signal product watch, `Claude#63044` as secondary product watch, `opencode#29699` as upstream-owned PR cover, and `opencode#29692` as assigned Desktop project-identity watch. No upstream comment/PR.
