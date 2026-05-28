# Claude/opencode/Codex watch, 2026-05-28 09:43 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562723960)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this used the documented fallback: parent live GitHub gates, reused 6 read-only subagent roles, and focused duplicate/PR checks before status selection.

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Runner gate: dedicated `corp-opensource-runner` is still unavailable via [#10](https://github.com/serejaris/corp-opensource/issues/10). CT216 remains Hermes-only and was not used.

## Material deltas

- `anthropics/claude-code#63061`: closed at `2026-05-28T09:38:55Z` after duplicate bot linked `#63054/#61043/#62350`. Status: `NO-GO / duplicate-closed`.
- `sst/opencode#29713`: merged at `2026-05-28T09:40:17Z`. Status: `WATCH / settled`.
- `openai/codex#24886`: fresh reasoning-only `/statusline` enhancement, no comments. Status: `WATCH`.
- `sst/opencode#29714`: fresh Desktop GUI wrong-workspace issue, assigned `Hona`; repo-local issue/PR searches for `wrong workspace` and `workspace path` returned no exact cover. Status: `WATCH / assigned-lead`, not `COMMENT-FIRST`.
- `anthropics/claude-code#63063`: fresh Cmd+V image paste feature request; product enhancement. Status: `WATCH`.
- `anthropics/claude-code#63062`: closed needs-info / minimal repro. Status: `NO-GO`.

Carry-forward unchanged:

- `louislva/claude-peers-mcp#64` remains the best compact source-level `CANDIDATE-needs-runner-validation`.
- `anthropics/claude-code#63055` remains candidate-shaped and duplicate-light, but likely API/product-owned `WATCH`; no upstream action without source/process/runner gates.
- `vercel/ai#15652`, `probelabs/probe#568`, and `google-gemini/gemini-cli#27503` remain runner-backed candidates but had no transition.
- `router-for-me/CLIProxyAPI#3594` remains protocol-heavy `LEAD`.
- `sst/opencode#29710/#29712` and `modelcontextprotocol/typescript-sdk#2163` remain active PR covers; `langchain#37723` remains contributor-intent race.

## Parent live gates

`claude-code#63061`:

- State: closed.
- Duplicate bot links: `#63054`, `#61043`, `#62350`.
- Decision: `NO-GO / duplicate-closed`.

`opencode#29713`:

- State: merged.
- Merged at `2026-05-28T09:40:17Z`.
- Scope: ACP-next warm switch performance/cache PR.
- Decision: `WATCH / settled`.

`codex#24886`:

- State: open, label `enhancement`.
- Body: asks for a reasoning-only `/statusline` item separate from existing model and model-with-reasoning items.
- Decision: `WATCH`; feature request, not bug lane.

`opencode#29714`:

- State: open, assigned `Hona`.
- Body: Desktop GUI selected `E:\Code\...\operate-backend` but reused server-side workspace `D:\Code\...\operate-backend`; both paths exist, so agent may operate in the wrong repo.
- Repo-local duplicate/PR search via `gh issue/pr list --search` for `wrong workspace` and `workspace path` returned no exact cover.
- Decision: `WATCH / assigned-lead`; assignee and desktop/session complexity block `COMMENT-FIRST`.

`claude-code#63055`:

- State: open, labels `bug`, `has repro`, `platform:macos`, `api:anthropic`, `area:routines`.
- Duplicate/race role found no exact cover and called it `CANDIDATE / COMMENT-FIRST`, but process/actionability rejected upstream action because this is likely product/API-owned and lacks source/repro gates.
- Decision: product `WATCH`.

## 6-role synthesis

- Fresh discovery found `opencode#29714` as the only new candidate-shaped post-09:36 issue, plus weak/product feature deltas `codex#24886`, `claude#63063`, and no-go `claude#63062`.
- Active refresh found `claude#63061` closed and `opencode#29713` merged; core candidates did not move.
- Duplicate/race classified `claude#63061` as duplicate-closed, `opencode#29713` as settled, `codex#24886` as low-priority feature watch, and `claude#63055` as duplicate-light but product/API gated.
- Process gates rejected `COMMENT-FIRST` and `PR-READY`: mandatory skills unavailable, runner unavailable, CT216 Hermes-only, no current-main repro/regression card, assigned opencode lane, product-owned Claude/Codex surfaces.
- Actionability keeps `claude-peers-mcp#64` as the top compact source-level candidate once runner validation is available.
- Synthesis selected `next_status: WATCH`.

## 3-role critique

- Factology/duplicates: approved internal watch update only; material transitions are duplicate close, merge, feature watch, and assigned opencode lead.
- Process gates: upstream comment/PR count stays `0`.
- Actionability: no runner work. Next promotion still requires runner-backed validation for `claude-peers-mcp#64` or another source-level lane with full duplicate/process gates.
