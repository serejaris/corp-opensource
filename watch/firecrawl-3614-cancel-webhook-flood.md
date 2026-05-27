# Firecrawl #3614 - crawl cancellation webhook flood

Date: 2026-05-27

## Upstream

- Issue: https://github.com/firecrawl/firecrawl/issues/3614
- Existing minimal PR: https://github.com/firecrawl/firecrawl/pull/3615
- Existing broader PR: https://github.com/firecrawl/firecrawl/pull/3623
- Triage comment: https://github.com/firecrawl/firecrawl/issues/3614#issuecomment-4551066501

## Subagent Synthesis

Verdict: real bug, good repo fit, but no fresh PR now.

The bug is localized: when a crawl is cancelled, `scrape-worker.ts` detects the cancelled parent crawl and throws `JobCancelledError`, but the catch path still sends per-page failure webhooks. This produces cancellation-induced page failure events for queued/unprocessed URLs.

However, two open PRs already link `Fixes #3614`:

- `#3615`: minimal one-file guard in `scrape-worker.ts`, skipping per-URL webhook on cancellation.
- `#3623`: broader API semantics change, adding a terminal `crawl.cancelled` event and suppressing page failures.

Maintainer signal is not clear yet. No new PR should be opened until maintainers pick the desired behavior.

## Test Surface

The useful contribution angle is a focused regression test, not a third patch PR.

A test can prove the bug without live scraping:

- mock `getCrawl()` to return a cancelled crawl;
- pass a single-url job with `crawl_id` and webhook config;
- mock `createWebhookSender().send`;
- assert no `WebhookEvent.CRAWL_PAGE` failure is sent for `JobCancelledError`.

Relevant files:

- `apps/api/src/services/worker/scrape-worker.ts`
- `apps/api/src/controllers/v1/crawl-cancel.ts`
- `apps/api/src/controllers/v2/crawl-cancel.ts`
- `apps/api/src/services/worker/crawl-logic.ts`
- `apps/api/src/services/webhook/types.ts`

## Repo/PR Notes

- `CONTRIBUTING.md` says normal fork/change/test/PR flow.
- Main API validation: `cd apps/api && pnpm harness pnpm test:snips`.
- CI `test-server.yml` uses the same snips harness.
- Recent titles use `fix(api): ...` or `fix(api/<area>): ...`.

## Decision

No fresh PR. Keep as `duplicate-triage` / `upstream-watch`.

Possible next action only after maintainer signal:

- add a targeted regression test to `#3615` if maintainers prefer minimal suppression;
- or add tests to `#3623` if maintainers prefer a terminal `crawl.cancelled` event.
