# Continue #12334 OpenRouter autocomplete extraBodyProperties

Date: 2026-05-27

## Upstream

- Issue: https://github.com/continuedev/continue/issues/12334
- Tracker: https://github.com/serejaris/corp-opensource/issues/50
- Upstream plan comment: https://github.com/continuedev/continue/issues/12334#issuecomment-4554636607
- Upstream evidence comment: https://github.com/continuedev/continue/issues/12334#issuecomment-4554830611

## Problem

Autocomplete requests for OpenRouter models do not include configured `requestOptions.extraBodyProperties`, so users cannot disable or hide reasoning tokens for autocomplete models.

Reporter config shape:

```yaml
provider: openrouter
roles:
  - autocomplete
requestOptions:
  extraBodyProperties:
    reasoning:
      exclude: true
      enabled: false
      effort: none
```

The regular request path can carry extra body properties, but the autocomplete path appears to drop them.

## Duplicate gate

- `gh search prs --repo continuedev/continue "12334 OR OpenRouter extraBodyProperties autocomplete OR extraBodyProperties autocomplete OR reasoning exclude enabled effort"` -> no results.
- Issue timeline has no linked/cross-referenced PR except the internal `corp-opensource#50` tracker cross-reference.
- Issue has no assignee.
- Our upstream plan comment is posted.

Latest live check: 2026-05-27 13:14 UTC.

- `continuedev/continue#12334` is still open and unassigned.
- No linked/closing upstream PR found.
- Adjacent PRs exist, but none directly cover `OpenRouter + autocomplete + extraBodyProperties`.

## Contribution gate

Continue `CONTRIBUTING.md` says to open a new issue or comment on an existing one before writing code. Done.

PR template asks for:

- description;
- checklist;
- screenshot/recording when applicable;
- tests.

## Regression card

- Contract: the autocomplete request path must preserve configured `requestOptions.extraBodyProperties` for OpenRouter.
- Test target: core autocomplete request/provider path.
- Expected failure if bug reproduces: captured outbound autocomplete body lacks `reasoning`.
- Expected pass: outbound body includes the configured `reasoning` object.
- Secrets: none. Use a mocked/captured fetch/request body.

## Local validation

Status: `WATCH / needs reporter path`.

Current upstream `main` at `cb273098d968906d25ee737b454f0b5f13ea2482` did not reproduce the suspected request-body drop on the tested path.

What was tested:

- Checkout: `/Users/ris/Documents/GitHub/continue`.
- Branch: `12334-openrouter-autocomplete-extra-body` tracking `upstream/main`.
- Runtime: Node `v20.20.1` via `npx -p node@20.20.1 -p npm@10`.
- Built local monorepo packages needed for core tests:
  - `packages/config-types`;
  - `packages/config-yaml`;
  - `packages/fetch`;
  - `packages/openai-adapters`;
  - `packages/llm-info`.
- Temporary targeted Vitest shape:
  - local HTTP server captures outbound body;
  - `new OpenRouter({ requestOptions.extraBodyProperties.reasoning })`;
  - `openrouter.streamComplete("const value =", signal, { raw: true })`;
  - assertion checks captured body includes `reasoning`.

Command result:

```bash
npx -y -p node@20.20.1 -p npm@10 npm run vitest -- llm/llms/OpenRouter.vitest.ts
```

Result: passed with the temporary targeted test.

Interpretation:

- Do not open a code PR yet: current `main` already preserves `extraBodyProperties.reasoning` on the tested raw completion/autocomplete-like path.
- The bug may be fixed on `main` but absent from a released extension, or the reporter is hitting a different autocomplete path.
- Upstream evidence comment asks for Continue version/build and exact outbound request path/body.

## Local checkout

Target checkout: `/Users/ris/Documents/GitHub/continue`.

Branch target: `12334-openrouter-autocomplete-extra-body`.

First clone attempt failed with transient GitHub HTTP 502 / `expected packfile`; later shallow/filter clone succeeded.

Current checkout is clean and has no PR diff.
