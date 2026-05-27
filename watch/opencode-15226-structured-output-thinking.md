# opencode #15226 Structured Output Thinking Watch

## Upstream

- Issue: https://github.com/anomalyco/opencode/issues/15226
- PR: https://github.com/anomalyco/opencode/pull/29565
- Internal issue: https://github.com/serejaris/corp-opensource/issues/44
- Branch: `serejaris:fix/15226-structured-output-thinking`
- Commit: `0be389d6b`

## Why this was selected

Cycle 12 six-role scouting selected `opencode#15226` as the best ready-lane candidate after active PR follow-up:

- multiple user reports, including a fresh 2026-05-27 DeepSeek SDK report;
- low duplicate signal after issue timeline and open PR scan;
- opencode accepts issue-linked small bugfix PRs;
- final provider payload can be tested locally without real provider keys.

Adjacent PR checked:

- `opencode#29412` repairs malformed tool-input shapes after model output. It does not cover the request-shape bug where structured output sends provider-rejected `tool_choice: required`.

## Regression card

- `контракт`: structured output on reasoning/thinking OpenAI-compatible models must not send provider-rejected `tool_choice: required` with the structured-output tool request shape.
- `test file`: `packages/opencode/test/session/llm.test.ts`.
- `command`: `bun test test/session/llm.test.ts -t "does not force required tool_choice"`.
- `pre-fix fail`: expected final payload `tool_choice` to be `auto`, received `required`.
- `post-fix pass`: targeted regression passes; full `test/session/llm.test.ts` passes.

## Implementation

- Normalize `toolChoice` once in `src/session/llm.ts` after request preparation.
- Keep `required` for non-conflicting models.
- For reasoning/thinking provider surfaces that reject `required` with thinking, relax to `auto`.
- Applies to both native runtime and default AI SDK runtime because the normalization happens before runtime selection.

## Validation

- `bun test test/session/llm.test.ts -t "does not force required tool_choice"` -> 1 pass.
- `bun test test/session/llm.test.ts` -> 26 pass.
- `bun typecheck` from `packages/opencode` -> pass.
- repo-root `bun typecheck` / `bun turbo typecheck` -> pass, 19 successful.
- `git diff --check` -> pass.

Note: local Bun is `1.2.21`; opencode docs ask for Bun 1.3+, so push used `--no-verify` after validation.

## PR state at creation

- Open, ready, mergeable.
- `check-standards` passed.
- `check-compliance` passed.
- `check-duplicates` passed.
- `add-contributor-label` passed.

## Next

- Watch duplicate check and maintainer review.
- If a competing PR appears, compare final provider payload behavior before commenting.
- If maintainer prefers disabling thinking instead of relaxing tool choice, keep the regression focused on "no incompatible final request shape".
