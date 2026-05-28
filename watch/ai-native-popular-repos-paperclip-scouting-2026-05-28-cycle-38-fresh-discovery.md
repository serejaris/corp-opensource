# Fresh AI-native issue discovery cycle 38, 2026-05-28

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Selected candidate tracker: [#86](https://github.com/serejaris/corp-opensource/issues/86)

Related repo-scope tracker: [#66](https://github.com/serejaris/corp-opensource/issues/66)

Tracker comments: [#52 cycle 38](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560779972), [#66 Vercel follow-up](https://github.com/serejaris/corp-opensource/issues/66#issuecomment-4560780593).

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this cycle used the documented fallback: parent live GitHub gates, targeted duplicate PR search, 6 read-only roles, then 3-role critique.

No upstream PR/comment/ping was made.

## Scope

Fresh-discovery slice over popular AI-native / terminal-agent / MCP / browser-agent repos:

- `QwenLM/qwen-code#4579`
- `PrefectHQ/fastmcp#4241`
- `modelcontextprotocol/inspector#1368`
- `ChromeDevTools/chrome-devtools-mcp#2027`
- `vercel/ai#15652`
- `browser-use/browser-use#4801`

## Decision

`next_status: CANDIDATE`

Selected internal candidate: `vercel/ai#15652`, tracked in [#86](https://github.com/serejaris/corp-opensource/issues/86).

Upstream action count: `0`.

Runner action count: `0`.

Runner gate remains closed via [#10](https://github.com/serejaris/corp-opensource/issues/10); CT216 remains Hermes-only.

## Parent Live Gates

| Lane | Live result | Decision |
|---|---|---|
| `vercel/ai#15652` | Open, unassigned, no labels/comments. No exact PR found for Anthropic `structuredOutputMode: "jsonTool"` hardcoding `disableParallelToolUse: true` and ignoring user override `false`. Adjacent provider/tool PRs such as `#15112`, `#10274`, `#10812`, and `#12903` remain watch risks. | Selected internal `CANDIDATE`. Package-level runner fixture required before any upstream action. |
| `modelcontextprotocol/inspector#1368` | Open, unassigned, labeled `v2`, no comments. Targeted PR search found no exact per-call/session-scoped UI reset on disconnect. | Reserve `CANDIDATE / WATCH`; strong bug but heavier V2 UI/state runner burden than Vercel package fixture. |
| `QwenLM/qwen-code#4579` | Open, assigned to `doudouOUC`, labels `type/bug`, `category/core`, `scope/session-management`. Exact open PR [#4580](https://github.com/QwenLM/qwen-code/pull/4580) is non-draft, `MERGEABLE/BLOCKED`, review-required, and says `Closes #4579`. | `WATCH / duplicate-covered`; no fresh PR lane. |
| `browser-use/browser-use#4801` | Open, bug label, no assignee. Exact/super-strong open PRs [#4880](https://github.com/browser-use/browser-use/pull/4880) and [#4835](https://github.com/browser-use/browser-use/pull/4835) both claim to fix icon-only/accessibility-label behavior for `#4801`. | `WATCH / duplicate-covered`; no fresh PR lane. |
| `PrefectHQ/fastmcp#4241` | Open, label `build failed`. Repeated action comments show broad upgrade-check failures; external comment isolates one `prefab-ui` subfailure but explicitly does not claim to fix broader `ty` diagnostics. Open maintainer work around `ty` is adjacent/partial. | `WATCH / process-gated`; no cold PR without assignment or tighter failure split. |
| `ChromeDevTools/chrome-devtools-mcp#2027` | Open, `collecting-feedback`. Maintainer reproduced and stated `fill()` sets the value correctly while the app handles it incorrectly; `type_text` is the workaround, and reporter confirmed. | `WATCH / weak candidate`; needs new independent test page or maintainer ask before re-entry. |

## 6-Role Synthesis

- Factology: `Qwen#4579` and `browser-use#4801` are duplicate-covered by active PRs and must not be treated as implementation targets.
- Duplicate/race: `vercel/ai#15652` and `inspector#1368` had no exact PR cover in this cycle. Vercel has heavier provider/tool adjacency; Inspector has V2 UI churn adjacency.
- Process: `FastMCP#4241` is too broad for cold action and has assignment/process pressure. `ChromeDevTools#2027` has maintainer guidance that weakens a core-bug claim.
- Actionability: `vercel/ai#15652` is the best runner target from this slice because it can start as a request-serialization fixture, without external Anthropic credentials.
- Reserve: `inspector#1368` remains a real candidate, but requires Node/browser/UI state repro and is more volatile around V2.

## 3-Role Critique

- Factology/duplicates: selected `vercel/ai#15652` is defensible; `#15112` is Bedrock-specific and not an exact Anthropic `jsonTool` cover. `Qwen#4579` and `browser-use#4801` must be recorded as duplicate-covered.
- Process: creating one dedicated internal tracker [#86](https://github.com/serejaris/corp-opensource/issues/86) is allowed because `#66` is repo-scope and this cycle selected one concrete lane. Upstream action remains forbidden.
- Actionability: do not overclaim the fix. A runner fixture can prove current request serialization ignores the user override; it does not prove maintainers must accept override semantics for synthetic JSON-tool behavior.

## Next Gates

1. Fresh clone/current `main` in `corp-opensource-runner` with Node/pnpm matching `vercel/ai` requirements.
2. Repeat duplicate search for `#15652`, `disableParallelToolUse`, `jsonTool`, `structuredOutputMode`, and Anthropic tool calling.
3. Failing package-level fixture proving request serialization currently sends `disable_parallel_tool_use: true` despite `disableParallelToolUse: false` on the `jsonTool` path.
4. Contribution/checks/changeset gate.
5. 3-role pre-action critique before any upstream comment or PR.
