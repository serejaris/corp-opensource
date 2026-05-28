# Pydantic/Vercel/Codex covered refresh 2026-05-28 08:16

Дата: 2026-05-28

Tracker:

- Umbrella: https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562007431

Статус: `WATCH`

Upstream actions: `0`

Runner actions: `0`

## Коротко

Bounded refresh после `f919b77` / tracker heartbeat `2026-05-28T08:08:08Z` не нашёл нового чистого PR target. Material deltas:

- `pydantic/pydantic-ai#5663` got active draft PR cover `#5670`.
- `vercel/ai#15664` already has targeted PR `#15665`.
- `openai/codex#24475` got a fresh confirming comment, but remains product/app-server watch.

## Parent live gates

`pydantic/pydantic-ai#5663`: https://github.com/pydantic/pydantic-ai/issues/5663

- State: open.
- Updated: `2026-05-28T08:09:41Z`.
- Labels: `docs`, `new models`, `xai`, `pydanty:model-request`.
- Assignee: `colesmcintosh`.
- Comments: `3`.
- Material delta: pydanty says the xAI Grok 4.3 / 4.20 docs, `KnownModelName`, profile, and tests are PR-ready.

Cover PR: `pydantic/pydantic-ai#5670`: https://github.com/pydantic/pydantic-ai/pull/5670

- State: open, draft.
- Author: `colesmcintosh`.
- Assignee: `dsfaccini`.
- Mergeability: `CONFLICTING`.
- Updated: `2026-05-28T08:09:56Z`.
- Decision impact: issue is owner/author-covered, not our candidate.

`vercel/ai#15664`: https://github.com/vercel/ai/issues/15664

- State: open.
- Created/updated: `2026-05-28T07:32:54Z`.
- Assignee: none.
- Comments: `0`.
- Report: `@ai-sdk/google-vertex` serializes unsupported `functionCall.id` and `functionResponse.id` fields into Vertex native request bodies.

Cover PR: `vercel/ai#15665`: https://github.com/vercel/ai/pull/15665

- State: open, non-draft.
- Author: `onmax`.
- Mergeability: `MERGEABLE`.
- Review decision: `REVIEW_REQUIRED`.
- Updated: `2026-05-28T08:08:48Z`.
- Decision impact: exact targeted PR exists, so no competing work.

`openai/codex#24475`: https://github.com/openai/codex/issues/24475

- State: open.
- Updated: `2026-05-28T08:11:17Z`.
- Labels: `bug`, `CLI`, `app`, `connectivity`, `subagent`, `app-server`.
- Assignee: none.
- Comments: `5`.
- Material delta: a new commenter reports similar subagent-tool hang/reconnect symptoms on Codex `134`; earlier comments already distinguish this from duplicate bot candidate `#23043`.
- Decision impact: strong product signal, but no OSS patch lane or runner-backed repro path in this repo.

## Duplicate / race

- `pydantic-ai#5663`: exact live cover is draft PR `#5670`; maintainer previously invited `colesmcintosh` to contribute and the reporter accepted.
- `vercel/ai#15664`: exact live cover is open PR `#15665` by issue author `onmax`.
- `codex#24475`: duplicate bot suggested `#23043`, but reporter disputed the duplicate and differentiated macOS/App+CLI reconnect loop from Windows/TUI tool-discovery failures. No exact PR cover found in this cycle, but the surface is Codex Desktop/AppServer product-owned and not runner-actionable here.

## 6-role synthesis

Новые subagent threads недоступны из-за thread limit, поэтому цикл переиспользовал шесть уже перечисленных agents.

- Fresh discovery found no strong created-after-`08:08Z` issues in the scoped high-signal repos; material signals were updated/race deltas.
- Active refresh found no post-cutoff close/merge/review/maintainer-request transitions in current watch/PR lanes.
- Duplicate/race critique kept `opencode#29698` as `NO-GO/WATCH` due bot duplicate hints `#17279/#17940` and assignment, and kept `CLIProxyAPI#3592` gated by `#2746`.
- Actionability critique still ranks `vercel/ai#15652`, `probelabs/probe#568`, and `google-gemini/gemini-cli#27503` as the top runner-backed candidate backlog, but runner `#10` remains unavailable.
- Process critique blocks upstream comment/PR: required skills are unavailable, runner gate is open, and the material fresh deltas are already covered or product-owned.

## Decision

`next_status: WATCH`

Do not open upstream PR/comment. Track `pydantic-ai#5670` and `vercel/ai#15665` as author-owned cover PRs; watch `codex#24475` only for maintainer signal or an OSS-facing reproducible source path.
