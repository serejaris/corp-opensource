# Claude Cowork/OTel/image watch, 2026-05-28 10:10 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562948899)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this used the documented fallback: parent live GitHub gates, reused 6 read-only subagent roles, and focused duplicate/PR checks before status selection.

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Runner gate: dedicated `corp-opensource-runner` is still unavailable via [#10](https://github.com/serejaris/corp-opensource/issues/10). CT216 remains Hermes-only and was not used.

## Material deltas

- `anthropics/claude-code#63075`: fresh Windows Desktop/Cowork crash on every message, labels `bug`, `platform:windows`, `area:cowork`, `area:desktop`; no exact PR cover found, but it sits in a broad Cowork Windows VM/crash cluster. Status: product `WATCH / LEAD`, not candidate.
- `anthropics/claude-code#63074`: fresh OTel logs missing `trace_id`/`span_id` while spans correlate, labels `bug`, `area:core`; search found strong existing open issue `#55269` for the same OTel log correlation gap. Status: `WATCH / duplicate-adjacent`.
- `anthropics/claude-code#63071`: fresh Windows/PowerShell image paste regression, labels `bug`, `platform:windows`, `area:tui`; search found open adjacent image-paste issues `#32791`, `#57440`, `#58923`, and `#12644`. Status: product `WATCH / duplicate-risk`.
- `anthropics/claude-code#63072`: duplicate-labeled thinking-block API error and body points to `#12311`. Status: `NO-GO / duplicate`.
- `anthropics/claude-code#63073`: Norwegian localization feature, now duplicate-labeled/enhancement. Status: `NO-GO / duplicate-feature`.
- `sst/opencode#29716`: feature request for per-turn Desktop diff viewer, closed by reporter as already existing within one minute. Status: `NO-GO / self-closed`.

Carry-forward unchanged:

- `louislva/claude-peers-mcp#64` remains the best compact source-level `CANDIDATE-needs-runner-validation`; exact PR search for `max_tokens max_completion_tokens gpt-5.4-nano` still found no open PR.
- `vercel/ai#15652`, `probelabs/probe#568`, and `google-gemini/gemini-cli#27503` remain runner-backed candidates with no exact PR cover found in this pass.
- `router-for-me/CLIProxyAPI#3594` remains protocol-heavy `LEAD/WATCH`.
- `langchain-ai/langchain#37728` remains `WATCH / duplicate-covered` by `langchain-ai/langchain-google#1704/#1708`.

## Parent live gates

`claude-code#63075`:

- State: open, no assignee/comments.
- Body: Desktop Cowork crashes immediately on every message, including a blank `hello`; no Cowork or VM logs are generated, and `AppData\Roaming\Claude` is absent.
- Search found broad adjacent Cowork Windows VM/crash/open issues including `#42119`, `#55649`, `#59298`, `#27801`, and closed historical crash lanes.
- Decision: product/Desktop `WATCH / LEAD`; no upstream action without logs, maintainer direction, and Windows/Cowork repro.

`claude-code#63074`:

- State: open, no assignee/comments.
- Body: OTel log events such as `api_request_body`, `user_prompt`, and `tool_decision` ship empty `trace_id`/`span_id` even though sibling spans correlate correctly.
- Duplicate search found open `#55269`: OTel log records emitted without `trace_id`/`span_id`.
- Decision: `WATCH / duplicate-adjacent`; no racing comment/PR.

`claude-code#63071`:

- State: open, no assignee/comments.
- Body: Portuguese Windows/PowerShell report that Ctrl+V image paste stopped working in CLI `2.1.153`.
- Duplicate search found multiple open/closed Windows image-paste lanes, including `#32791`, `#58923`, `#57316`, `#42440`, and `#50552`.
- Decision: product `WATCH / duplicate-risk`; weak repro and product surface block promotion.

`opencode#29716`:

- State: closed at `2026-05-28T10:06:49Z`; reporter comment says it already exists.
- Decision: `NO-GO / self-closed`.

## 6-role synthesis

- Fresh discovery found `claude-code#63075` and `#63071` as the only post-10:02 items stronger than plain feature noise, but both are Claude product-owned surfaces and need stronger maintainer/repro context before any action.
- Duplicate/race classified `#63074` as covered by old open `#55269`; `#63072/#63073` are duplicate-labeled; opencode `#29716` is closed.
- Active refresh found no material changes for `claude-peers-mcp#64`, `vercel/ai#15652`, `probe#568`, `gemini-cli#27503`, `CLIProxyAPI#3594`, or LangChain Google duplicate cover.
- Process/actionability roles kept final `next_status: WATCH`; runner absence and missing regression cards block `COMMENT-FIRST`/`PR-READY`.

## 3-role critique

- Factology/duplicates: internal watch update is allowed, but do not overclaim Claude product issues as source-level candidates; `#63074` has a strong existing duplicate.
- Process gates: upstream comment/PR count stays `0`; no runner action.
- Actionability: next promotion still requires runner-backed validation for a source-level lane such as `claude-peers-mcp#64`, `vercel/ai#15652`, `probe#568`, or `gemini-cli#27503`.
