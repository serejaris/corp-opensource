# Vercel AI #15663 onFinish/onEnd docs

Дата: 2026-05-28

Upstream: https://github.com/vercel/ai/issues/15663

Trackers:

- Umbrella: https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561245208
- Vercel: https://github.com/serejaris/corp-opensource/issues/86#issuecomment-4561246297

Статус: `LEAD`

Upstream actions: `0`

Runner actions: `0`

## Коротко

`vercel/ai#15663` сообщает, что после merged `#15245` и canary-линии AI SDK 7 часть docs/cookbook examples всё ещё использует deprecated `onFinish` внутри прямых `streamText({ ... })` вызовов. Current `main` подтверждает, что `streamText` поддерживает `onEnd`, а `onFinish` оставлен как deprecated alias.

Это валидный docs-consistency lead, но не runtime/tool-call candidate. Он ниже `vercel/ai#15652`, `probelabs/probe#568`, `google-gemini/gemini-cli#27503` и `langchain4j/langchain4j#5313`.

## Parent live gates

- Issue: open, unassigned, labels absent, comments absent at gate time.
- Repo: `vercel/ai`, ~24.5k stars, active `main`, `CONTRIBUTING.md` present, GitHub license detection `Other`.
- Source evidence: affected docs/cookbook/provider MDX files still contain `streamText({ ... onFinish ... })` examples. The issue's caveat is correct: `toUIMessageStreamResponse({ onFinish })` is a different callback surface and should not be swept.
- Duplicate/PR search: no exact open PR cover for `#15663`, `onFinish -> onEnd`, or the affected docs paths.
- Neighboring PRs: `#14772` touches `content/cookbook/01-next/73-mcp-tools.mdx` but does not cover the lifecycle callback rename. `#15245` is the merged source/regression context, not a cover.
- Runner: not required for initial lead triage, but any upstream PR would need current-main source sweep, docs/lint validation, and a process/legal check for the repo license.

## 6-role synthesis

Existing runtime candidates had no material transition: `probe#568`, `gemini-cli#27503`, `vercel/ai#15652`, `langchain4j#5313`, `AionUi#3074`, and `continue#12507` remain in their prior statuses. Active PR dashboard had no merge/close/review/maintainer-ask transition. `corp-opensource-runner` remains unavailable via `#10`.

Fresh discovery after `2026-05-28T05:55:00Z` found no stronger high-signal repo issue beyond `vercel/ai#15663`.

## Critique

- Factology: pass. `#15245` renamed the final lifecycle callback; current source/docs support the deprecation framing. This applies to AI SDK 7 canary/current `main`, not stable v6.
- Duplicate/race: pass for now. No exact open PR cover; nearby path PRs are only adjacent.
- Process/actionability: keep as `LEAD`. It is docs-only, lower strategic weight than runtime/tool-call lanes, and issue-author follow-up may create self-PR race. No upstream comment/PR.

## Decision

`next_status: LEAD`

Keep as opportunistic micro-docs watch. Recheck only if no author/maintainer PR appears and the primary runner backlog stalls.

## Follow-up, 2026-05-28 06:26 UTC

Live gate update: issue remains open, unassigned, unlabeled. The earlier gate statement "comments absent at gate time" is now stale: the issue has 3 comments, all from the issue author. The author offered to open a PR and expanded the affected surface beyond docs/cookbook MDX files:

- `packages/mcp/README.md`
- several `examples/ai-functions/src/**` files that still demonstrate `onFinish`
- filename-level examples using `on-finish` that would need an `on-end` rename if the example contract changes

Duplicate/PR search still found no exact open PR cover for `#15663`, `onFinish -> onEnd`, `examples/ai-functions`, or the author `n-satoshi061`. This does not lower the race risk: the issue author has effectively claimed a possible fix lane even without an opened PR.

6-role fallback refresh: factology, duplicate/race, and process-gate roles agree that this should stay below runtime/tool-call candidates. Runner is still unavailable via corp tracker `#10`, but this lane is docs/examples consistency rather than a runner-first repro.

Decision: `next_status: LEAD/WATCH`, upstream actions `0`, runner actions `0`.

Do not open a competing upstream PR now. Recheck only if maintainer asks for an external PR, the issue author explicitly drops ownership, or no author/maintainer PR appears after a reasonable wait and primary runtime candidates are blocked.
