# CopilotKit#4911 - workspace protocol leaked into published package

Date: 2026-05-27

## Links

- Upstream issue: https://github.com/CopilotKit/CopilotKit/issues/4911
- Upstream PR: https://github.com/CopilotKit/CopilotKit/pull/5035
- Internal issue: https://github.com/serejaris/corp-opensource/issues/19
- Fork branch: https://github.com/serejaris/CopilotKit/tree/codex/fix-4911-workspace-deps
- Local checkout: `/Users/ris/Documents/GitHub/CopilotKit-4911`

## Status

`PR OPEN / REVIEW_REQUIRED`.

This became the next PR candidate after the six-agent scouting cycle on 2026-05-27 because it had:

- clear user-facing install failure;
- low duplicate risk;
- small patch surface;
- existing release-script test surface;
- no maintainer invitation gate.

## Bug

`@copilotkit/web-inspector@1.57.2` was published with:

```json
"@copilotkit/core": "workspace:*"
```

Downstream pnpm consumers outside the CopilotKit workspace fail with `ERR_PNPM_WORKSPACE_PKG_NOT_FOUND`.

`npm view` confirmed on 2026-05-27:

- `@copilotkit/web-inspector@1.57.2` still has `@copilotkit/core: workspace:*`.
- latest `@copilotkit/web-inspector@1.58.0` has `@copilotkit/core: 1.58.0`, but `main` still had a regression test preserving the wrong release-script behavior.

## Fix

Changed `scripts/release/lib/versions.ts` so shared-version release bumps replace internal dependencies even when the source manifest uses `workspace:*`.

Changed `scripts/release/lib/versions.test.ts` to assert publishable package manifests receive the release version instead of preserving `workspace:*`.

## Verification

Pre-fix regression proof:

```bash
pnpm exec vitest run scripts/release/lib/versions.test.ts
```

Failed with:

```text
expected 'workspace:*' to be '1.55.3'
```

Post-fix checks:

```bash
pnpm exec vitest run scripts/release/lib/versions.test.ts
pnpm exec oxfmt --check scripts/release/lib/versions.ts scripts/release/lib/versions.test.ts
pnpm exec oxlint scripts/release/lib/versions.ts scripts/release/lib/versions.test.ts
```

Commit hook also completed successfully:

- `pnpm test`
- package checks: `publint`, `attw`

## PR state

Checked after opening PR #5035:

- state: open;
- ready, not draft;
- mergeable;
- review decision: `REVIEW_REQUIRED`;
- Claude review disabled because PR is from a fork;
- Vercel preview statuses show `FAILURE` because a CopilotKit team member must authorize fork deployments. Treat this as maintainer authorization pending, not a code failure.

## Next action

- Watch maintainer review and CI.
- If asked, clarify that Vercel failures are fork deployment authorization.
- If maintainers prefer `^${newVersion}` instead of exact `newVersion`, adjust release-script test and implementation together.
