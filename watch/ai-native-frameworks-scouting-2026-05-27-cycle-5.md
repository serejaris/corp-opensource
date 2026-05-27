# AI-native frameworks scouting cycle 5

Дата: 2026-05-27

Метод: active PR follow-up + duplicate triage по следующему queued candidate.

## Active PR/watch status

| Item | Live status | Action |
|---|---|---|
| [goose#9136](https://github.com/aaif-goose/goose/issues/9136) | Open, unassigned, no maintainer reply yet to assignment ask | Wait; no PR before green light |
| [opencode#29530](https://github.com/anomalyco/opencode/pull/29530) | Open, mergeable; duplicate/compliance/standards green | Wait maintainer review |
| [OpenHands SDK#3394](https://github.com/OpenHands/software-agent-sdk/pull/3394) | Open, review required; statusCheckRollup empty | Wait maintainer review/checks |
| [cline#11087](https://github.com/cline/cline/pull/11087) | Open, mergeable; Quality Checks + Ubuntu/Windows SDK tests green | Wait maintainer review |
| [CopilotKit#5035](https://github.com/CopilotKit/CopilotKit/pull/5035) | Open, mergeable; docs preview green; other Vercel previews blocked by team authorization | Wait maintainer/auth |
| [pydantic-ai#5678](https://github.com/pydantic/pydantic-ai/pull/5678) | Open, mergeable, full CI green | Wait maintainer review |
| [pydantic-ai#5680](https://github.com/pydantic/pydantic-ai/pull/5680) | Open, mergeable, full CI green | Wait maintainer review |

## Duplicate triage: pydantic-ai #5419

Upstream issue: https://github.com/pydantic/pydantic-ai/issues/5419

Existing PR: https://github.com/pydantic/pydantic-ai/pull/5443

Internal tracker: https://github.com/serejaris/corp-opensource/issues/29

Decision: `WATCH / duplicate-covered-with-failing-ci`.

Why:

- #5419 is a real bug: with multiple `MCPServerTool`s, only the last `mcp_list_tools` emits a result, leaving AG-UI stuck on earlier discovery calls.
- #5443 already targets exactly this behavior: `fix(openai): prevent AG-UI stalls during multi-server MCP discovery`.
- #5443 is open and mergeable, but not merge-ready.
- Maintainer asked PR author to use VCR cassettes recorded from real API traffic instead of hand-crafted histories.
- CI currently fails only at `coverage`/final `check`.

Coverage failure detail:

```text
pydantic_ai_slim/pydantic_ai/models/openai.py    1702      2    820      2  99.84%   3284-3285, 3440->3272
TOTAL                                           86589      2  12492      2  99.99%
Coverage failure: total of 99.99 is less than fail-under=100.00
```

## Lesson

For streaming adapter bugs, synthetic event histories can be insufficient. If maintainers ask for VCR or real API cassette evidence, the regression must prove the real wire stream shape. A hand-crafted sequence may test desired mapper behavior but fail to prove whether the upstream stream omitted chunks or the adapter dropped them.

## Next

1. Do not open a competing PR for pydantic-ai #5419.
2. Watch #5443 for author update, maintainer assignment, or stalled state.
3. If maintainers ask for help, useful contribution is likely:
   - VCR cassette / real API traffic repro;
   - coverage fix for `openai.py` missing lines/branch;
   - review comment comparing event-stream vs completed-response behavior.
