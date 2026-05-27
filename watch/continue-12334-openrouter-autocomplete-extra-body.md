# Continue #12334 OpenRouter autocomplete extraBodyProperties

Date: 2026-05-27

## Upstream

- Issue: https://github.com/continuedev/continue/issues/12334
- Tracker: https://github.com/serejaris/corp-opensource/issues/50
- Upstream plan comment: https://github.com/continuedev/continue/issues/12334#issuecomment-4554636607

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
- Pre-fix expected failure: captured outbound autocomplete body lacks `reasoning`.
- Post-fix expected pass: outbound body includes the configured `reasoning` object.
- Secrets: none. Use a mocked/captured fetch/request body.

## Local checkout

Target checkout: `/Users/ris/Documents/GitHub/continue`.

Branch target: `codex/12334-openrouter-autocomplete-extra-body`.

First clone attempt failed with transient GitHub HTTP 502 / `expected packfile`; a shallow retry was stopped cleanly when the user asked to pause. No partial checkout remains.
