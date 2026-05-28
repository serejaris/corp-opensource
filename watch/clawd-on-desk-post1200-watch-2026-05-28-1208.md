# clawd-on-desk post-12:00 watch, 2026-05-28 12:08 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Обязательные skills `open-source-bug-scouting` / `open-source-pr-workflow` в этом окружении всё ещё не установлены. Цикл выполнен через documented fallback: 6 read-only subagent ролей, parent live gates и focused 3-role critique.

Upstream comment, PR, ping, rerun или runner action не выполнялись.

`next_status: CANDIDATE`

Fresh delta: `rullerzhou-afk/clawd-on-desk` = repo-scope `WATCH`.

Upstream action count: `0`.

Runner action count: `0`.

## Parent live gates

Runner gate остаётся закрытым через [#10](https://github.com/serejaris/corp-opensource/issues/10): dedicated `corp-opensource-runner` не provisioned. CT `216` остаётся Hermes-only и не использовался для general repro.

GitHub Search API частично вернул `403 rate limit`, поэтому duplicate/race coverage для части broad/focused searches неполный. Direct issue, PR и repo views по записанным lanes были доступны.

| Lane | Live result | Decision |
|---|---|---|
| `rullerzhou-afk/clawd-on-desk` | Новый repo-scope сигнал. На момент live gate: public, non-archived JavaScript repo; AGPL-3.0; `3,138` stars, `323` forks, pushed `2026-05-28 12:02:46 UTC`. Repo metadata positions it as a desktop companion/pet that watches Claude Code, Codex, Cursor, Gemini and other AI coding agents. Topics include `claude-code`, `codex`, `copilot`, `cursor`, `desktop-pet`, `electron`, `gemini`. Local README/watch search found no prior `clawd-on-desk` entry. | Repo-scope `WATCH`; relevant desktop/session companion surface, but no bounded issue, source scan, repro, process pass, or full duplicate/race gate. |
| `rullerzhou-afk/clawd-on-desk#352/#355` | `#352` is open and unassigned, with an owner comment at `2026-05-28 12:03:33 UTC` splitting the report. Owner PR `#355` is open and covers the Ghostty same-cwd tab focusing slice in `src/focus.js` plus tests. | `WATCH/NO-GO for Ghostty same-cwd focus`; owner-handled slice, not our candidate lane. |
| `openai/codex#24906` | Fresh open bug created `2026-05-28 12:01:35 UTC`: MCP OAuth Google-backed servers never get a refresh token. Focused issue search showed a larger existing OAuth refresh cluster including `#17265`, `#13852`, `#14144`, `#13956`, and `#21407`; focused PR search returned no open cover before Search API limit. | Product/app-auth `WATCH`; duplicate-clustered, not candidate. |
| `nexu-io/open-design#3202` | Open bug/good-first/help-wanted issue got `/claim` at `2026-05-28 12:03:45 UTC`; open PR queue is already review-heavy. | `WATCH/NO-GO`; process/claim-gated, do not race. |
| `anthropics/claude-code#63107/#63108` | Fresh product feature requests for semantic/session search and tagging/filtering. | `WATCH`; product/support features, no public source-local lane. |
| `anomalyco/opencode#29571` | Open and assigned to `kitlangton`; fresh comment notes a manual Esc/revert workaround. Duplicate bot had linked `#21668`. | `WATCH/NO-GO`; assigned and duplicate-risk. |

Carry-forward backlog stayed open and uncovered but did not transition: `probe#568`, `claude-peers-mcp#64`, `google-gemini/gemini-cli#27503`, `vercel/ai#15652`, `simular-ai/Agent-S#195`, `browseros-ai/BrowserOS#1005`, `generalaction/emdash#1875`, `topoteretes/cognee#2907`, `agentmemory#703-#708`, `OpenViking#2256`, `cmux`, and `open-design`.

## 6-role synthesis

- Fresh discovery resurfaced already-tracked strong lanes such as `playwright-mcp#1635`, `vercel/ai#15430/#15652`, `react-grab#417`, `crush#3024`, and `mcp-use#1581`; these are older or already covered by local watch context and do not change current action gates.
- Active refresh found no state/assignee/label/comment transition that makes a carry-forward lane `COMMENT-FIRST` or `PR-READY`. `open-design#3202` got claimed by another contributor and is no-go for us.
- Duplicate/race guard kept all current lanes at `WATCH/CANDIDATE`: Codex/Claude issues are product or duplicate-clustered, opencode lanes are assigned/duplicate-risk, Vercel/Gemini/Probe still need runner or source fail-before evidence.
- Process gates confirmed upstream action is not allowed: runner unavailable, Codex external PR path is invitation-only, Gemini process expects help-wanted/community assignment path, and several lanes are assignment/claim/owner-gated.
- Actionability kept no-runner source-first order as `claude-peers-mcp#64`, `Agent-S#195`, partial `gemini-cli#27503`, then Vercel/cognee/OpenViking source passes; true runner order starts with `probe#568`.
- Scope/materiality selected `rullerzhou-afk/clawd-on-desk` as the only new repo-scope material delta after the 12:00 cmux watch.

## 3-role critique

- Factology/duplicates: record `clawd-on-desk` only as repo-scope `WATCH`; star/fork counts are live-gate snapshots, AGPL-3.0 is a future process/licensing risk, and repo positioning is from metadata, not source reading. `#352` is not a candidate; `#355` is an owner PR for the narrow Ghostty same-cwd focus slice, not a global duplicate clearance. GitHub Search `403` means exact duplicate/race search must be repeated before any upstream action.
- Process gates: mandatory-skill fallback is a process risk, not a full substitute. Internal watch/tracker update is allowed; upstream action remains blocked by no selected bounded lane, no runner/source evidence, and no pre-action gates.
- Actionability: keep repo discovery separate from the candidate queue. Existing source-first candidates remain `claude-peers-mcp#64`, `Agent-S#195`, partial `gemini-cli#27503`; first full runner target remains `probe#568`.

## Next

Keep `rullerzhou-afk/clawd-on-desk` on repo-scope watch. A future focused pass should choose exactly one bounded desktop/session/focus/control-plane issue, then run source scan, contribution/process check, and fresh issue/PR duplicate search before any upstream comment or PR.
