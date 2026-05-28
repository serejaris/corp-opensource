# Fresh AI-native watch, 2026-05-28 08:48 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this used the documented fallback: parent live GitHub gates, reused 6 read-only subagent roles, and focused duplicate/PR checks before status selection.

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Runner gate: dedicated `corp-opensource-runner` is still unavailable via [#10](https://github.com/serejaris/corp-opensource/issues/10). CT216 remains Hermes-only and was not used.

## Material deltas

- `google-gemini/gemini-cli#27517`: duplicate bot added `status/possible-duplicate` at `2026-05-28T08:42:18Z`, pointing to `#27506/#27366/#21835/#27504/#10617/#10523/#27516/#27501/#11502`. Keep `WATCH / NO-GO for now`.
- `anomalyco/opencode#29704`: fresh OpenCode Desktop Windows/Intel iGPU animation performance issue. The issue is already assigned to `Hona`; duplicate bot points to `#21939` and asks the reporter to clarify GPU/CPU impact versus visual preference. Keep `WATCH / duplicate-assignee gated`.
- `mem0ai/mem0#5284`: fresh open PR for `Memory.search()` LangChain vector store `None` score crash. It is non-draft, `MERGEABLE`, `REVIEW_REQUIRED`, and includes three regression tests. This is already externally covered, so local status is `WATCH`.
- `ag-ui-protocol/ag-ui#1801`: fresh Kotlin Interrupts types enhancement. The issue body says the author will create a PR, so keep `WATCH / author-intent race`.
- `anthropics/claude-code#62959`: fresh external workaround/recovery comment for Windows session transcript deletion/ghost sidebar entries. It is useful product intel, not an external patch lane.
- `openai/codex#24457`: fresh external analysis comment about coupling custom provider `base_url` with the credential source. Existing duplicate bot points to `#23659`; keep product `WATCH`.

## Parent live gates

`gemini-cli#27517`:

- State: open.
- Labels: `status/need-triage`, `area/core`, `status/possible-duplicate`.
- Bot duplicate list: `#27506/#27366/#21835/#27504/#10617/#10523/#27516/#27501/#11502`.
- Decision: weak `WATCH / duplicate-risk`, no upstream action.

`opencode#29704`:

- State: open; assignee: `Hona`.
- Created: `2026-05-28T08:45:24Z`; updated: `2026-05-28T08:47:05Z`.
- Body: OpenCode Desktop thinking/sub-agent animations can drive older Intel iGPU usage to about `20%` average and sometimes above `50%`.
- Duplicate bot: points to `#21939` for disabling/reducing animations and asks whether this is separate due GPU/CPU impact.
- Focused PR search for animation/GPU terms returned no cover.
- Decision: `WATCH`; assigned and duplicate-gated.

`mem0#5284`:

- State: open PR, non-draft, `MERGEABLE`, `REVIEW_REQUIRED`.
- Fixes `#5071` by guarding `None` score before `score_and_rank` threshold comparison.
- PR body reports `pytest tests/utils/test_scoring.py -v` with `21 passed`.
- Decision: already covered by external PR; watch only.

`ag-ui#1801`:

- State: open issue, label `enhancement`, no comments/assignee.
- Body says Interrupts are no longer draft and Kotlin SDK lacks types; author says they will create a PR.
- Decision: `WATCH / author-intent race`.

`claude-code#62959`:

- State: open, labels `bug`, `platform:windows`, `data-loss`, `area:desktop`.
- Fresh comment suggests Windows Volume Shadow Copies and recovery tooling for deleted transcripts; explicitly says the root cause is upstream.
- Decision: product `WATCH`.

`codex#24457`:

- State: open, labels `bug`, `windows-os`, `auth`, `custom-model`, `app`, `config`, `remote`.
- Existing bot duplicate: `#23659`.
- Fresh comment proposes resolving provider name, `base_url`, wire API, auth requirement, and credential source as one provider context.
- Decision: product `WATCH / duplicate-risk`.

## 6-role synthesis

- Fresh discovery added `mem0#5284`, `ag-ui#1801`, `opencode#29704`, and product-watch comments in Claude/Codex.
- Active refresh found `gemini-cli#27517` moved to possible-duplicate and `opencode#29704` became assigned/duplicate-gated; no material transition for `claude-peers-mcp#64`, `CLIProxyAPI#3594`, `claude-code#63050`, `codex#24884`, `pydantic-ai#5670`, `vercel/ai#15665/#15652`, or `probe#568`.
- Duplicate/race kept `claude-peers-mcp#64` and `CLIProxyAPI#3594` as uncovered source-level lanes, but `CLIProxyAPI#3594` is still protocol-heavy and `claude-peers-mcp#64` still needs runner validation.
- Process gates rejected `COMMENT-FIRST` and `PR-READY` for all lanes because mandatory skills are unavailable, runner `#10` is unavailable, and fresh lanes are duplicate-covered, assigned, author-intent, or product-owned.
- Actionability keeps `claude-peers-mcp#64` as the best compact source-level `CANDIDATE-needs-runner-validation`; runner backlog order remains `vercel/ai#15652`, `probelabs/probe#568`, `gemini-cli#27503`.
- Synthesis selected `next_status: WATCH` because this cycle only adds watch/no-go deltas and no candidate promotion.

## 3-role critique

- Factology/duplicates: approved after adding parent live gates for subagent-discovered `mem0#5284`, `ag-ui#1801`, `claude-code#62959`, and `codex#24457`.
- Process gates: approved internal watch/tracker update only. Upstream PR/comment count stays `0`.
- Actionability: no runner work. Next promotion still requires runner-backed validation for `claude-peers-mcp#64` or an existing runner backlog candidate once `corp-opensource-runner` is provisioned.
