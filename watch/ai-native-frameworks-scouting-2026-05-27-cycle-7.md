# AI-native Frameworks Scouting Cycle 7

Checked at `2026-05-27T08:03Z`.

## Scope

Follow-up cycle over active upstream PRs before starting new scouting. Goal: avoid stale internal state, stale duplicate assumptions, and unnecessary new PRs while existing branches are waiting for review.

## Live PR State

| Target | Live state | Action |
|---|---|---|
| [pydantic-ai#5678](https://github.com/pydantic/pydantic-ai/pull/5678) | Open, mergeable, full CI green; only Codex review quota warning | Watch maintainer/bot review |
| [pydantic-ai#5680](https://github.com/pydantic/pydantic-ai/pull/5680) | Open, mergeable, full CI green; only Codex review quota warning | Watch maintainer/bot review |
| [cline#11087](https://github.com/cline/cline/pull/11087) | Open, mergeable; Greptile gap addressed in `e1ecc63a1`; Quality Checks + Ubuntu/Windows SDK tests green | Watch maintainer review |
| [opencode#29530](https://github.com/anomalyco/opencode/pull/29530) | Open, mergeable; duplicate/compliance/standards checks green | Watch maintainer review |
| [OpenHands/software-agent-sdk#3394](https://github.com/OpenHands/software-agent-sdk/pull/3394) | Open, mergeable, review required; no status checks reported | Watch maintainer review/checks |
| [CopilotKit#5035](https://github.com/CopilotKit/CopilotKit/pull/5035) | Open, mergeable, review required; docs preview green; other Vercel previews blocked by team authorization | Watch maintainer/auth |
| [Hermes#32999](https://github.com/NousResearch/hermes-agent/pull/32999) | Open but conflicting; duplicate PR race active | Wait maintainer consolidation before rebase/noise |

## Decisions

- No new upstream PR in this cycle; active PR queue already has several review gates.
- Cline README status was stale and has been corrected from `needs repro before PR` to `PR open, green, waiting review`.
- The OpenHands SDK PR belongs to `OpenHands/software-agent-sdk`, not `OpenHands/OpenHands`; use the full owner/repo in future checks to avoid false closed-state reads.
- Reactions, bot summaries, and review-tool comments are evidence to inspect, not maintainer approval.

## Testing Lesson

Cline #11087 showed a reusable provider-routing rule:

- after fixing the reported `thinking=false` path, review found the mirrored `thinking=true` path could still leak provider options;
- the follow-up regression must cover both disabled and enabled modes when a compatibility bug is about strict endpoint payload shape;
- validation should assert the final outgoing `providerOptions` payload, not only helper predicates.

## Next

1. Keep watching active PRs for maintainer review/merge/CI changes.
2. Before opening another PR, first clear any actionable review/failure on existing branches.
3. If no active PR needs action, continue scouting from issue #11/#13 with fresh duplicate scans.
