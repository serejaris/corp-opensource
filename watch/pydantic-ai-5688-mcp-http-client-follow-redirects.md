# pydantic-ai #5688: MCPToolset http_client factory kwargs

Date: 2026-05-27
Upstream: https://github.com/pydantic/pydantic-ai/issues/5688
Internal issue: https://github.com/serejaris/corp-opensource/issues/47
Local checkout: `~/Documents/GitHub/pydantic-ai-5688`
Local branch: `codex/5688-mcp-http-client-follow-redirects`

## Status

WATCH / duplicate-covered by `pydantic/pydantic-ai#5694`. Не отправлять upstream PR/comment от нас, пока открыт покрывающий PR.

Upstream comment offer already posted:

https://github.com/pydantic/pydantic-ai/issues/5688#issuecomment-4554005692

## Follow-up 2026-05-27

Live gates:

- `pydantic/pydantic-ai#5688` остаётся open и unassigned с labels `bug`, `MCP`, `httpx`, `pydanty:bug`.
- `pydanty` подтвердил root cause и пометил issue как PR-ready, но maintainer assignment или confirmation для нас нет.
- Наш upstream offer comment был условным: внешний PR только если maintainers хотят этого и нет активного pydanty/delegated opener.
- `pydantic/pydantic-ai#5694` открыт, non-draft, exact covering PR для FastMCP HTTP factory kwargs / `follow_redirects`; visible CI green/skipped, `mergeStateStatus=CLEAN`, `mergeable=MERGEABLE`, review decision пока не записан.
- Смежный `#5664` — `httpx2` prep, не exact fix для `#5688`.

6-role scouting fallback и 3-role critique выбрали ровно один status:

`next_status: WATCH`

Decision: local regression-first patch и verification остаются только historical evidence. Сейчас не делать duplicate PR и не оставлять повторный upstream comment. Мониторить review/merge/close по `#5694`.

Re-entry condition: возвращаться к `COMMENT-FIRST` или `PR-READY` только если `#5694` закроют без merge, maintainers явно попросят alternative patch/regression, или merged fix не покроет `MCPToolset(url, http_client=...)` `follow_redirects` path.

Runner: новый runner repro в этом цикле не нужен, потому что duplicate gate блокирует upstream action раньше repro. Повторять current-main repro в `corp-opensource-runner` только после re-entry condition.

## Contract

`MCPToolset(url, http_client=...)` adapts a user-provided `httpx.AsyncClient` into FastMCP's `httpx_client_factory`.

That factory must tolerate extra kwargs FastMCP passes at runtime, including `follow_redirects=True`, and still return the user-provided client.

## Regression proof

Test file:

`tests/test_mcp_toolset.py`

Command:

```bash
uv run pytest tests/test_mcp_toolset.py -k "http_client_kwarg_uses_factory or sse_url_with_http_client_uses_factory"
```

Pre-fix result after adding the regression assertions:

```text
TypeError: _make_httpx_client_factory.<locals>.factory() got an unexpected keyword argument 'follow_redirects'
```

This failed for both streamable HTTP and SSE transport construction.

## Patch

Patch surface:

`pydantic_ai_slim/pydantic_ai/mcp.py`

Minimal implementation:

```python
def factory(
    headers: dict[str, str] | None = None,
    timeout: httpx.Timeout | None = None,
    auth: httpx.Auth | None = None,
    **_kwargs: Any,
) -> httpx.AsyncClient:
    return http_client
```

Regression assertions go through public construction:

```python
toolset = MCPToolset('https://example.com/mcp', http_client=client)
httpx_client_factory = cast(Callable[..., httpx.AsyncClient], toolset.client.transport.httpx_client_factory)
assert httpx_client_factory(follow_redirects=True) is client
```

SSE has the same assertion.

## Verification

Targeted tests:

```text
uv run pytest tests/test_mcp_toolset.py -k "http_client_kwarg_uses_factory or sse_url_with_http_client_uses_factory"
2 passed, 84 deselected
```

Lint:

```text
uv run ruff check pydantic_ai_slim/pydantic_ai/mcp.py tests/test_mcp_toolset.py
All checks passed!
```

Typecheck:

```text
PYRIGHT_PYTHON_IGNORE_WARNINGS=1 uv run pyright pydantic_ai_slim/pydantic_ai/mcp.py tests/test_mcp_toolset.py
0 errors, 0 warnings, 0 informations
```

## Lesson

For user-supplied client/callback/factory adapter bugs, the first regression must go through the public constructor/config API and call the callback shape used by the dependency. A helper-only test is not enough.

This rule was added to `/Users/ris/.codex/skills/open-source-pr-workflow/SKILL.md`.
