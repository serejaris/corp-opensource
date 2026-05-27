# Upstream Follow-up Loop

## Зачем

Open-source contribution не заканчивается на PR. Нужно регулярно проверять старые PR/issues, потому что:

- maintainer мог закрыть PR как duplicate;
- появился competing PR;
- наш PR мог стать stale/conflicting;
- upstream мог merge соседний fix, который нужно сравнить;
- мы могли забыть ответить людям, которые помогли или сообщили новый edge case.

## Frequency

- Active upstream PR: проверять минимум раз в день, а во время активной гонки PR — чаще.
- Merged/closed PR: проверить через 24-48 часов, затем на release follow-up.
- Watch issue with maintainer question: проверять до ответа или stale/no-go decision.

## What To Check

For each active upstream item:

```bash
gh pr view <number> --repo OWNER/REPO --json state,isDraft,mergeable,reviewDecision,statusCheckRollup,comments,reviews,commits,updatedAt,url
gh pr checks <number> --repo OWNER/REPO
gh api repos/OWNER/REPO/issues/<number>/comments
```

Also search competing work:

```bash
gh search prs --repo OWNER/REPO "<core bug terms>" --state open --limit 20
gh search issues --repo OWNER/REPO "<core bug terms>" --state open --limit 20
```

## Comment Rules

Write upstream comments when there is new useful information:

- competing PR appears and we compared behavior;
- maintainer asked a question;
- CI/checks are blocked by maintainer approval or fail for a reason;
- our branch is still distinct from a newer PR;
- we can offer to port tests or rebase after another PR lands;
- a user reports success/failure with our fix and the maintainers should see the signal.

Do not comment just to bump.

## Required Internal Record

Every check updates the relevant `corp-opensource` issue/watch note with:

- date/time;
- upstream state;
- competing PRs/issues checked;
- whether an upstream comment was posted;
- next check/action.

## Pre-Push Test Lesson

If an upstream repo has strict coverage gates, do not rely only on a targeted `-k` regression run before force-push. Run the smallest full owner test file/module that can cover all new branches, and make the regression include every data shape touched by the patch.

Example from `pydantic-ai#5678`: the targeted binary serialization test passed, but upstream coverage failed at `99.99` because tuple handling lines were not exercised. The fixed regression now covers mapping bytes and tuple/list nested bytes before push.

## Example: Hermes Codex Null Output

On 2026-05-27:

- #32963 merged the canonical outage fix.
- #32884 was closed as duplicate.
- #32999 remained open for a distinct callback-error hardening regression.
- #33017 appeared as a usage-preservation follow-up touching overlapping files.
- Action: commented on #32999 explaining that #33017 and #32999 are complementary and offering to rebase if #33017 lands first.
