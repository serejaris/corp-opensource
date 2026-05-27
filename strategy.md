# Open Source Contribution Strategy

## Goal

Build a repeatable system for finding high-signal bugs in important repositories, fixing them fast, and turning the process into reputation, learning, and reusable agent workflows.

## Target Repo Profile

Prefer repositories that are:

- actively used by AI builders, developers, or infra teams;
- updated in the last 7 days;
- large enough that a fix is visible, but not so bureaucratic that first contribution takes weeks;
- runtime/tooling products where we can reproduce real bugs locally;
- friendly to focused bugfix PRs with tests.

Avoid as primary targets:

- list-only repos, book collections, roadmaps, or pure docs catalogs;
- repos with unclear maintainership or no recent merged PRs;
- giant corporate repos where small external PRs drown unless issue is already well scoped.

## Candidate Lanes

| Lane | Why it fits | Example repos |
|---|---|---|
| AI agents / coding agents | Strong fit with our daily tooling and fast outage detection | `NousResearch/hermes-agent`, `anthropics/claude-code`, `anomalyco/opencode`, `google-gemini/gemini-cli` |
| AI app/runtime frameworks | Bugs affect many builders, often reproducible with examples | `langchain-ai/langchain`, `langflow-ai/langflow`, `langgenius/dify`, `open-webui/open-webui` |
| Data/document conversion | Clear fixtures and regression tests | `microsoft/markitdown`, `firecrawl/firecrawl` |
| Developer UI/tools | High impact, but only pursue small scoped bugs | `microsoft/vscode`, `shadcn-ui/ui`, `excalidraw/excalidraw` |
| Self-hosted products | Good for Docker/runtime bugs and release follow-up | `n8n-io/n8n`, `supabase/supabase`, `immich-app/immich` |

## Bug Discovery Loop

1. Search live issue/PR streams for fresh pain:
   - recent issues with `bug`, `regression`, `crash`, `TypeError`, `NoneType`, `cannot`, `fails`, `broken`;
   - PRs closed as stale without fix;
   - duplicate issues with many reactions;
   - release regressions after a new version.
2. Score candidates:
   - reproducible locally: +3
   - affects our own workflow: +3
   - many reactions/comments/duplicates: +2
   - likely small patch with test: +2
   - maintainers active in last 48h: +2
   - requires huge domain context: -3
   - no test harness or hard external dependency: -2
3. Dispatch subagents:
   - Repo rules: contribution guide, tests, PR template, maintainer norms.
   - Bug evidence: issue history, duplicates, user impact, reproduction.
   - Patch surface: files, existing tests, likely minimal fix.
4. Only start implementation when there is:
   - a concrete repro or failing test;
   - a scoped patch surface;
   - a plausible targeted validation command.

## Weekly Operating Rhythm

- Monday: refresh candidate repos and open/close tracking issues.
- Daily: check active upstream watches and comments.
- For each accepted bug: create a `duplicate-triage` issue if competing PRs exist.
- After every PR outcome: update `watch/`, skill examples, and labels.

## Success Metrics

- PRs opened.
- PRs merged or fixes landed upstream.
- Bugs reproduced with tests.
- Maintainer interactions.
- Issues where our duplicate triage changed the outcome.
- Lessons added to `/Users/ris/.codex/skills/open-source-pr-workflow`.

