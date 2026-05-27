# OpenHands#14476 - conversation metadata race hides started resolver conversations

Date: 2026-05-27

## Links

- Upstream issue: https://github.com/OpenHands/OpenHands/issues/14476
- Internal issue: https://github.com/serejaris/corp-opensource/issues/21

## Status

`WATCH / CANDIDATE`.

No open or closed competing PR was found via `gh search prs` for:

- `14476`
- `conversation metadata`
- `started resolver`

## Bug

The issue reports that GitHub resolver jobs start real runtimes/conversations, but some conversations do not appear in the user's visible conversation list.

The reported evidence points to a race between:

- start path saving `AppConversationInfo`;
- webhook path saving `AppConversationInfo` for the same `conversation_id`.

The result can be duplicate-key failures on `conversation_metadata` and SaaS metadata assertion failures.

## Why It Matters

This is an agent-runtime reliability bug: work can start and consume runtime resources while disappearing from the user-facing control plane.

It is relevant to Paperclip/Hermes-style task runners because metadata persistence must be idempotent and race-safe.

## Risk

This may require OpenHands SaaS metadata semantics and production DB context. Do not open a drive-by PR unless a local unit test can prove the duplicate-save behavior and the expected merge/update contract.

## Next Action

Only pursue after higher-confidence candidates if:

1. the save service can be tested without SaaS/private infra;
2. duplicate `conversation_id` save can be reproduced with a deterministic unit test;
3. the fix is limited to idempotent insert/update or conflict handling, not broad org metadata policy.
