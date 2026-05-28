# Continue #12507 context provider link

Дата: 2026-05-28

Upstream: https://github.com/continuedev/continue/issues/12507

Tracker: https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4561177594

Статус: `LEAD`

Upstream actions: `0`

Runner actions: `0`

## Коротко

`continuedev/continue#12507` сообщает, что action `Add more context providers` в chat `@` dropdown открывает старый docs URL:

`https://docs.continue.dev/customization/context-providers#built-in-context-providers`

Reporter ожидает переход на актуальную страницу custom providers. На live gate current `main` всё ещё содержит старый URL в `gui/src/components/mainInput/TipTapEditor/utils/getSuggestion.ts`.

Это валидный и компактный UI/docs-link баг в крупном AI-native repo, но ниже текущего runner backlog: `probelabs/probe#568`, `google-gemini/gemini-cli#27503`, `vercel/ai#15652`, `langchain4j/langchain4j#5313`.

## Parent live gates

- Issue: open, unassigned, labels `area:chat`, `area:context-providers`, `area:docs`, `kind:bug`.
- Repo: `continuedev/continue`, Apache-2.0, ~33k stars, active `main`, `CONTRIBUTING.md` present.
- Source: current `main` has stale URL in `gui/src/components/mainInput/TipTapEditor/utils/getSuggestion.ts` near the `Add more context providers` action.
- Duplicate/PR search: exact open PR search for `12507`, `Add more context providers`, `getSuggestion`, and the stale docs URL found no cover. Broad `#12202` is docs-file-only and not a cover for the UI source link.
- Runner: not needed for initial `LEAD`, but any upstream PR should still verify the correct replacement URL and run Continue's relevant GUI lint/test path in runner or an equivalent stable environment.

## 6-role scouting synthesis

Fresh slice after `2026-05-28T05:34:00Z` found no stronger high-signal AI-native bug. Rejected fresh hits:

- `openai/codex#24863`: small bug, but Codex remains no-go without invitation.
- `anomalyco/opencode#29677`: billing/subscription issue, possible duplicate, not a code contribution candidate.

Active PR dashboard and existing runner backlog had no material transition. `corp-opensource-runner` remains unavailable via `#10`.

## 3-role critique

- Factology/duplicates: pass. Link target is stale/wrong; exact open PR cover not found. Note: live redirects may make the reporter's 404 symptom unstable, so avoid overclaiming the exact browser status.
- Process gates: pass for `LEAD` / possible future `CANDIDATE`; not `PR-READY` without replacement-URL confirmation and local/runner validation.
- Actionability/strategy: weak for primary backlog. This is a UI/docs-link paper cut, not agent-runtime evidence, so do not reorder runner targets.

## Decision

`next_status: LEAD`

Keep as opportunistic micro-fix/watch only. No upstream comment or PR in this cycle.
