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
- maintainer explicitly asks for clarification before implementation; answer that first and do not open a PR until the intended product direction is clear.

Before a new upstream comment/PR that changes candidate status, record the 3-subagent critique result in the issue/watch note.

Do not comment just to bump. Do not comment if there is no new evidence, the duplicate gate is incomplete, critique is not recorded, or the same question is already waiting for an answer.

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

## Recovery Boundary Test Lesson

For downstream recovery around dependency/SDK parser failures, write the first regression at the public runtime boundary and prove both allowed and forbidden recovery:

- allowed: the dependency failure that users hit is recovered through the normal stream/request path;
- forbidden: an app-local callback, handler, or post-processing exception with the same exception class/message is not swallowed.

Do not rely on a helper-only predicate test when the bug is the placement of `try/except` or the breadth of a recovery block. The test must fail before the fix because the real runtime recovered something it should have propagated.

## Example: Hermes Codex Null Output

On 2026-05-27:

- #32963 merged the canonical outage fix.
- #32884 was closed as duplicate.
- #32999 remained open for a distinct callback-error hardening regression.
- #33017 appeared as a usage-preservation follow-up touching overlapping files.
- Action: commented on #32999 explaining that #33017 and #32999 are complementary and offering to rebase if #33017 lands first.
- Later check: #32999 and #33017 both became conflicting while more duplicate PRs appeared. Action: no new upstream noise; keep the boundary regression as the reusable asset and wait for maintainer consolidation/rebase direction.

## Example: OpenHands Global Skills In Docker Serve

On 2026-05-27:

- OpenHands #14563 reported host-global skills not appearing in `openhands serve`.
- Six-agent triage found the loader already requests `load_user=True`; the likely break is Docker visibility: host `~/.openhands` is not mounted into the agent-server container.
- Related #14121/#14273 may be the intended Settings/personal-repo direction, so a runtime PR without maintainer direction risks design conflict.
- Action: posted a clarification/comment-first update and tracked internally in `corp-opensource#26`.

## Example: Goose Repeated Tool Calls

On 2026-05-27:

- goose #9136 was open, unassigned, and the snooze date had expired.
- Maintainer had already called it a real bug and gave a small one-shot fix plan.
- Six-agent triage found no active duplicate PR, but the maintainer wording said “before we pick this up”.
- Action: posted a comment-first assignment ask instead of opening a drive-by PR, and tracked internally in `corp-opensource#28`.
- Test lesson: for runtime guard bugs, helper-level coverage is insufficient. The regression must exercise the public guard path (`inspect()` over a batch of `ToolRequest`s), because the bug was stale state across requests, not the helper predicate itself.

## Example: Closing Stale Internal Candidates

On 2026-05-27:

- Internal `corp-opensource#21` still described OpenHands #14476 as a candidate with no competing PR.
- Fresh live check found OpenHands #14489 open/mergeable with regression tests and green Python/lint checks.
- Action: commented with exact PR/check evidence and closed the internal candidate as `duplicate-covered`.
- Rule: do not keep stale candidate issues open just because the bug is interesting. If upstream has sufficient active coverage, close or relabel internal work as watch-only.
