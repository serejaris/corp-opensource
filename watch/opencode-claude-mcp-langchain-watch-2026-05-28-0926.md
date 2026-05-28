# opencode/Claude/MCP/LangChain watch, 2026-05-28 09:26 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562611188)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this used the documented fallback: parent live GitHub gates, reused 6 read-only subagent roles, and focused duplicate/PR checks before status selection.

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Runner gate: dedicated `corp-opensource-runner` is still unavailable via [#10](https://github.com/serejaris/corp-opensource/issues/10). CT216 remains Hermes-only and was not used.

Search gate: GitHub Search API returned a transient rate-limit error during broad repo search, so this cycle used repo-local `gh issue list`, `gh pr list`, and direct `gh issue/pr view` gates. This blocks any upstream action.

## Material deltas

- `sst/opencode#29709`: merged at `2026-05-28T09:23:47Z`. Status: `WATCH / settled`.
- `sst/opencode#29711`: fresh CJK pasted-text issue, assigned `kommander`; duplicate bot links `#17032/#29707/#25854`. Status: `WATCH / duplicate-cluster`.
- `sst/opencode#29712`: fresh PR closing `#29711`, open, non-draft, `MERGEABLE`; converts display-width extmark offsets to JS char indices before slicing pasted text. Status: `WATCH / covered-by-PR`.
- `sst/opencode#29710`: still open, non-draft, `MERGEABLE`, updated at `09:23:56Z`; covers the adjacent wide-character paste corruption lane `#29707`.
- `sst/opencode#29708`: reporter clarified at `09:20:15Z` that `/init` question request rejection differs from bot-suggested `#27907`; still assigned `StarpTech`. Status: `LEAD/WATCH`, not action-ready.
- `anthropics/claude-code#63054`: fresh OSC 52/tmux clipboard regression with detailed WSL/devcontainer repro and labels `bug`, `has repro`, `area:tui`, `regression`, `platform:wsl`. Status: product `WATCH`; possible internal `CANDIDATE` only if public patch surface and duplicate gate become clear.
- `modelcontextprotocol/typescript-sdk#2163`: fresh PR fixing `#2162` invalid tool argument protocol errors; open, non-draft, `MERGEABLE`, `REVIEW_REQUIRED`, tests/checks listed. Status: `WATCH / covered-by-PR`.
- `langchain-ai/langchain#37723`: fresh `09:20Z` root-cause comment for structured-output Pydantic serializer warning says PR will be opened shortly. Status: `WATCH / contributor-intent-race`.

## Parent live gates

`opencode#29709`:

- State: `MERGED`.
- Merged at `2026-05-28T09:23:47Z`.
- Decision: `WATCH / settled`.

`opencode#29711/#29712`:

- `#29711` state: open, assigned `kommander`.
- Duplicate bot: `#17032`, `#29707`, `#25854`.
- `#29712` state: open PR, non-draft, `MERGEABLE`.
- PR body: closes `#29711`, converts display-width offsets to character indices for paste extmarks, includes verification claims.
- Decision: `WATCH / covered-by-PR`.

`opencode#29710/#29707`:

- `#29710` state: open PR, non-draft, `MERGEABLE`, updated `2026-05-28T09:23:56Z`.
- Covers `#29707`, adjacent to `#29711/#29712`.
- Decision: `WATCH / competing-covered-cluster`.

`opencode#29708`:

- State: open, assigned `StarpTech`, two comments.
- Bot duplicate hints: `#28112/#27907`.
- Reporter clarified that this is a first-`/init` delayed-answer lifecycle issue, not only busy-lockout.
- Decision: `LEAD/WATCH`; no upstream comment while assigned and duplicate/race gate is open.

`claude-code#63054`:

- State: open, labels `bug`, `has repro`, `area:tui`, `regression`, `platform:wsl`.
- Body includes regression window `2.1.146-2.1.153`, last known good `2.1.145`, WSL2/devcontainer/tmux repro, and sibling shell OSC 52 control.
- Open PR list has no obvious exact cover, but broad Search API gate was rate-limited.
- Decision: product `WATCH`, not upstream action.

`mcp/typescript-sdk#2162/#2163`:

- `#2162` state: open, label `bug`.
- `#2163` state: open PR, non-draft, `MERGEABLE`, `REVIEW_REQUIRED`.
- PR claims protocol error behavior for invalid tool args, regression test, typecheck, lint, `git diff --check`.
- Decision: `WATCH / covered-by-PR`.

`langchain#37723`:

- State: open, labels `bug`, `langchain`, `openai`, `external`, three comments.
- Latest comment identifies `_create_chat_result` / `model_dump(... parsed ...)` and says PR will be opened shortly.
- Decision: `WATCH / contributor-intent-race`.

## 6-role synthesis

- Fresh discovery found `claude-code#63054`, `mcp/typescript-sdk#2163`, `opencode#29711/#29712`, and `langchain#37723` as the only notable deltas.
- Active refresh found `opencode#29708` reporter clarification; core candidates `claude-peers-mcp#64`, `vercel/ai#15652`, `probe#568`, `gemini-cli#27503`, and `CLIProxyAPI#3594` had no material state change.
- Duplicate/race classified `opencode#29711/#29712` as duplicate/covered, `langchain#37723` as contributor-intent race, and `claude-code#63054` as interesting but requiring repeat duplicate/PR search.
- Process gates reject upstream action because mandatory skills are unavailable, GitHub Search gate was rate-limited, runner `#10` is unavailable, CT216 is Hermes-only, and no current-main repro/regression card exists.
- Actionability keeps `claude-peers-mcp#64` as the best compact source-level `CANDIDATE-needs-runner-validation`; runner backlog remains blocked.
- Synthesis selected `next_status: WATCH`.

## 3-role critique

- Factology/duplicates: approved internal watch update only; `opencode#29711/#29712` and MCP TS are already covered, `langchain#37723` has contributor intent, and `claude-code#63054` lacks complete duplicate/PR search due rate limit.
- Process gates: upstream comment/PR count stays `0`; no status-changing upstream action is allowed.
- Actionability: no runner work. Promotion still requires runner-backed validation for `claude-peers-mcp#64` or a later source-level lane with full duplicate/process gates.
