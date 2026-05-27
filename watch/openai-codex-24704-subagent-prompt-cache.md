# openai/codex#24704 - forked subagents lose prompt-cache lineage

Date: 2026-05-27

## Links

- Upstream issue: https://github.com/openai/codex/issues/24704
- Reference implementation from reporter: https://github.com/john-lu-ptc/codex/pull/1
- Related closed upstream PR: https://github.com/openai/codex/pull/20909
- Earlier closed upstream PR: https://github.com/openai/codex/pull/17248

## Status

`WATCH / duplicate-safe triage`.

This is in scope because it is directly about Codex subagents and inherited context cost. It affects the same class of workflows Ris wants for Paperclip/Hermes-style parallel agents.

Do not open a second PR yet. The reporter already supplied a reference implementation in their fork, and upstream has earlier closed attempts (#17248, #20909). The useful next step is to track whether OpenAI wants a fresh upstream PR or whether the reporter/reference branch is the intended source.

## Live evidence

Checked with `gh` on 2026-05-27:

- `openai/codex` is active: `86061` stars, pushed `2026-05-27T05:22:37Z`.
- Issue #24704 is open, labeled `bug`, `CLI`, `subagent`, created `2026-05-27T04:32:22Z`.
- The issue has no comments yet.
- Quick PR search did not find an open upstream PR for this exact bug.
- Reporter's PR `john-lu-ptc/codex#1` is open and includes both code and regression coverage.
- Upstream PR #17248 was opened on `2026-04-09` as `Inherit forked agent prompt cache keys`, then closed by stale automation on `2026-04-28`; it was not merged.
- Upstream PR #20909 was closed by stale automation on `2026-05-18`; it was not merged.
- `docs/contributing.md` says external PRs are currently invitation-only, so a no-invite upstream PR is likely process-wrong even if technically correct.

## Source audit

Current `main` in `/Users/ris/Documents/GitHub/openai-codex-24704` still derives request `prompt_cache_key` from the child thread id:

- `codex-rs/core/src/client.rs`: `let prompt_cache_key = Some(self.state.thread_id.to_string());`
- `codex-rs/core/src/session/session.rs`: `ModelClient::new(...)` receives `session_id` and `thread_id`, but no inherited prompt-cache key.

Existing nearby tests:

- `codex-rs/core/tests/suite/prompt_caching.rs`
- `codex-rs/core/src/session/tests.rs::fork_startup_context_then_first_turn_diff_snapshot`
- `codex-rs/core/tests/suite/subagent_notifications.rs::spawned_child_receives_forked_parent_context`
- `codex-rs/core/tests/suite/compact_resume_fork.rs`

## Regression test lesson

For this class of bug, a good PR must add a contract-level regression that proves the forked child request uses the parent prompt-cache lineage for full-history inherited context. A helper-level test is not enough.

Minimum useful proof:

1. Create parent session and seed one completed turn.
2. Flush/materialize the parent rollout.
3. Spawn a full-history forked subagent or forked thread.
4. Submit the first child turn.
5. Assert the captured `/responses` request has `prompt_cache_key == parent_thread_id`, while child thread/session identity remains separate.
6. Run the test against current `main` or reverted fix and capture the expected failure before treating it as regression coverage.

Best concrete insertion point from subagent triage:

- Extend `codex-rs/core/tests/suite/subagent_notifications.rs::spawned_child_receives_forked_parent_context`.
- Rename or extend it to assert:
  - parent/child request thread ids differ;
  - parent and child `body_json()["prompt_cache_key"]` match for `fork_context=true`;
  - child request still includes `TURN_0_FORK_PROMPT`;
  - child request still excludes `SPAWN_CALL_ID`.

Candidate targeted command if invited to implement:

```bash
just test -p codex-core spawned_child_receives_forked_parent_context_and_prompt_cache_key
```

## Next action

- Wait for maintainer/reporter signal on #24704 or a new upstream PR.
- If no upstream PR appears and maintainers invite one, use the reporter's test shape as prior art, but create a narrow upstream branch with explicit pre-fix regression failure evidence.
- Do not comment upstream unless adding concrete duplicate/coverage evidence; the issue already has a reference implementation.
