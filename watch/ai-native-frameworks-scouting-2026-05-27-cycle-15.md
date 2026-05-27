# AI-native frameworks scouting: cycle 15

Date: 2026-05-27
Mode: 6-subagent scouting after active upstream follow-up.

## Follow-up state

No active upstream PR needs a code fix right now:

- `anomalyco/opencode#29565`: open, ready, mergeable; opencode duplicate/compliance/standards checks green.
- `cline#11087`: open, mergeable; visible checks green; Greptile gap already addressed.
- `pydantic-ai#5680`: open, mergeable; CI/coverage green; Codex review quota warning only.
- `OpenHands/software-agent-sdk#3394`: open, mergeable; maintainer wants eval; old bot `CHANGES_REQUESTED` review remains in GitHub state, but follow-up comment/commit exists.
- `E2B#1354`: open and mergeable, but blocked by E2B CLA for `serejaris`.
- `google/adk-python#5864`: open; our comment-offer is latest actionable external comment; still `request clarification`.

## Outcome

Best technical target: `pydantic-ai#5688`.

Current action: comment-first/watch, not immediate PR, because Pydantic AI's current rules and automation make unassigned PRs risky and `pydanty` already wrote `PR-ready -> delegating to PR opener`.

## Primary candidate

- Upstream issue: https://github.com/pydantic/pydantic-ai/issues/5688
- Internal tracker: https://github.com/serejaris/corp-opensource/issues/47
- Comment-offer: https://github.com/pydantic/pydantic-ai/issues/5688#issuecomment-4554005692
- Status: open; labels `bug`, `MCP`, `httpx`, `pydanty:bug`.
- Duplicate scan: no linked/open PR found for the exact `MCPToolset(url, http_client=...)` + `follow_redirects` factory bug. Adjacent PR `#5664` is `httpx2` prep, not this closure signature.

### Regression card

- User contract: `MCPToolset(url, http_client=...)` must accept the user-provided `httpx.AsyncClient` when FastMCP transport passes extra kwargs such as `follow_redirects=True`.
- Patch surface: `pydantic_ai_slim/pydantic_ai/mcp.py`, inner factory returned by `_make_httpx_client_factory`.
- Test surface: likely `tests/test_mcp_toolset.py`.
- Command: `uv run pytest tests/test_mcp_toolset.py -k http_client_factory`.
- Expected pre-fix fail: `TypeError: ... factory() got an unexpected keyword argument 'follow_redirects'`.
- Secrets/platform: none if the regression directly exercises the factory/adapted callback.

## Other candidates

| Candidate | Decision | Reason |
|---|---|---|
| `google/adk-python#5864` | Watch / comment-first | No duplicate PR, but maintainer asked reporter to validate full test pass. |
| `cline#11065` | Watch / candidate after current Cline PR settles | Strong Windows plugin timeout candidate; maintainer says PR welcome, but we already have active `cline#11087`. |
| `microsoft/playwright-mcp#1635` | Wait approval | Strict approval/assignment gate; regression offer already posted. |
| `modelcontextprotocol/python-sdk#2689` | Watch | Good protocol bug, but needs repo-gate check and current duplicate scan before any PR. |
| `browser-use#4846` | Duplicate/watch | MCP CDP lifecycle likely covered by `browser-use#4881`. |
| `OpenHands#14563` | Comment-first | Strong skills/runtime issue, but product direction unresolved: host-global skills mount vs settings/personal repo. |
| `E2B#1352` | Watch/comment only | No public PR, but linked internal Linear; also our current E2B PR is CLA-blocked. |
| `trycua#1725` | Verify only | Current `main` may already cover click marker path; needs Windows smoke before code. |

## Decision

Do not open a new PR this cycle.

Next action:

1. Watch `pydantic-ai#5688` for maintainer assignment/confirmation or a pydanty-opened PR.
2. If greenlit and no competing PR exists, implement a narrow regression-first PR.
3. If pydanty opens the PR, compare regression coverage and comment only if the public `follow_redirects` factory contract is missing.

## Process lesson

`PR-ready` from a repo bot is not permission to race the bot or maintainer workflow. Treat it as a strong technical signal, then check the repo's social gate. If the issue says a PR opener is being delegated, leave a concise availability comment and wait.

## Local patch prep

Prepared a local regression-first branch without upstream submission:

- Local checkout: `~/Documents/GitHub/pydantic-ai-5688`
- Branch: `codex/5688-mcp-http-client-follow-redirects`
- Watch note: [pydantic-ai #5688 MCPToolset http_client follow_redirects patch prep](pydantic-ai-5688-mcp-http-client-follow-redirects.md)

Pre-fix failure after adding public-constructor regression:

```text
TypeError: _make_httpx_client_factory.<locals>.factory() got an unexpected keyword argument 'follow_redirects'
```

Post-fix verification:

```text
uv run pytest tests/test_mcp_toolset.py -k "http_client_kwarg_uses_factory or sse_url_with_http_client_uses_factory"
2 passed, 84 deselected

uv run ruff check pydantic_ai_slim/pydantic_ai/mcp.py tests/test_mcp_toolset.py
All checks passed!

PYRIGHT_PYTHON_IGNORE_WARNINGS=1 uv run pyright pydantic_ai_slim/pydantic_ai/mcp.py tests/test_mcp_toolset.py
0 errors, 0 warnings, 0 informations
```

Lesson added to `open-source-pr-workflow`: user-supplied client/callback/factory adapter bugs need a public constructor/config regression plus the actual downstream callback shape.
