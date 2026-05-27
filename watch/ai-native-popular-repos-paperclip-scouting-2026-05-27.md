# Popular AI-native / Paperclip-like scouting, 2026-05-27

Live state: GitHub-проверки через `gh` на 2026-05-27.
Upstream actions в этом проходе: 0 PR, 0 comments.
Tracker: [serejaris/corp-opensource#52](https://github.com/serejaris/corp-opensource/issues/52).

## Метод

Запрос: найти цели в популярных GitHub-репозиториях для Paperclip-like работы.

Что проведено:

- Цикл 1: шесть агентов на repo-fit, bug-signal, repro-path, patchability, duplicate-race, PR-readiness.
- Цикл 2: шесть агентов на deep dive по выбранным популярным AI-native repo и багам.
- Цикл 3: шесть агентов на финальную критику: fact-check, duplicate coverage, Paperclip strategy, PR-readiness, repro feasibility, actionability.
- Parent live gates проверили issue state, comments, linked/open PRs и явные duplicate lanes.

## Финальный вывод

| Priority | Upstream | Status | Почему важно для Paperclip | Следующее действие |
|---:|---|---|---|---|
| 1 | [trycua/cua#1724](https://github.com/trycua/cua/issues/1724) | `PR-READY after local macOS repro` | Computer-use driver startup, MCP stdio handshake, native crash isolation. | Воспроизвести `cua-driver mcp` SIGABRT на current `main` на macOS, потом решать, возможен ли узкий PR. |
| 2 | [OpenHands#14563](https://github.com/OpenHands/OpenHands/issues/14563) | `COMMENT-FIRST` | Skills discovery, Docker/CLI host-vs-runtime capability graph. | Ждать maintainer direction: CLI bind mount или Settings/personal-skills roadmap. |
| 3 | [modelcontextprotocol/python-sdk#2702](https://github.com/modelcontextprotocol/python-sdk/issues/2702) | `COMMENT-FIRST` | MCP server reliability, ASGI middleware boundary, Streamable HTTP lifecycle. | Ждать maintainer signal перед кодом; ожидаемый fix может быть docs/warning guard или runtime compatibility. |
| 4 | [cline/cline#10931](https://github.com/cline/cline/issues/10931) | `COMMENT-FIRST` | Terminal execution policy: pager detection, non-interactive defaults, stuck-command recovery. | Ждать maintainer narrowing: issue linked to Linear, scope шире одного простого patch. |
| 5 | [pydantic-ai#5688](https://github.com/pydantic/pydantic-ai/issues/5688) | `COMMENT-FIRST` | Маленький MCP client compatibility regression вокруг user-supplied `httpx` client factory. | Ждать pydanty/maintainer confirmation; local patch path уже подготовлен. |
| 6 | [playwright-mcp#1635](https://github.com/microsoft/playwright-mcp/issues/1635) | `COMMENT-FIRST` | Browser tool lifecycle: successful back navigation vs wait policy timeout. | Ждать assignment/direction; implementation может жить в `microsoft/playwright` core. |
| 7 | [continue#12382](https://github.com/continuedev/continue/issues/12382) | `COMMENT-FIRST` | Long-running agent identity и stale GitHub check/session cleanup. | Ждать owner direction: patch surface выглядит как backend/GitHub App lifecycle. |
| 8 | [anomalyco/opencode#29587](https://github.com/anomalyco/opencode/issues/29587) | `WATCH` | Instruction/context injection hygiene для terminal coding agents. | Watch only; issue assigned. |

## Repro feasibility

| Upstream | Feasibility | Suggested regression path |
|---|---|---|
| `pydantic-ai#5688` | Cheap local unit | `tests/test_mcp_toolset.py`; вызвать HTTP client factory с `follow_redirects=True` и проверить, что возвращается supplied client. |
| `MCP python-sdk#2702` | Container-feasible | `FastMCP("test").streamable_http_app()` плюс no-op `BaseHTTPMiddleware` и local `httpx.AsyncClient` initialize request. |
| `playwright-mcp#1635` | Browser/CDP needed | Playwright MCP back-navigation regression: `example.com -> iana.org -> back`, assert tool succeeds after committed URL. |
| `OpenHands#14563` | Docker/CLI needed | Host `~/.openhands/skills/<skill>/SKILL.md`, `openhands serve`, затем проверить visibility внутри agent-server/WebUI skills list. |
| `trycua#1724` | macOS required | Запустить `cua-driver mcp` на current `main`; зафиксировать SIGABRT before MCP handshake с native crash evidence. |

## Duplicate-covered / no fresh PR

| Upstream | Status | Blocking coverage |
|---|---|---|
| [browser-use#4846](https://github.com/browser-use/browser-use/issues/4846) | `WATCH` | Open PRs [#4847](https://github.com/browser-use/browser-use/pull/4847), [#4881](https://github.com/browser-use/browser-use/pull/4881). |
| [browser-use#4794](https://github.com/browser-use/browser-use/issues/4794) | `WATCH` | Open PRs [#4805](https://github.com/browser-use/browser-use/pull/4805), [#4831](https://github.com/browser-use/browser-use/pull/4831). |
| [continue#12450](https://github.com/continuedev/continue/issues/12450) | `WATCH` | Open PRs [#12451](https://github.com/continuedev/continue/pull/12451), [#12453](https://github.com/continuedev/continue/pull/12453). |
| [continue#12431](https://github.com/continuedev/continue/issues/12431) | `WATCH` | Open PR [#12437](https://github.com/continuedev/continue/pull/12437). |
| [cline#10139](https://github.com/cline/cline/issues/10139) | `WATCH` | Open PRs [#10140](https://github.com/cline/cline/pull/10140), [#10141](https://github.com/cline/cline/pull/10141), [#10384](https://github.com/cline/cline/pull/10384). |
| [cline#9784](https://github.com/cline/cline/issues/9784) | `WATCH` | Open PR [#10849](https://github.com/cline/cline/pull/10849). |
| [OpenHands#14476](https://github.com/OpenHands/OpenHands/issues/14476) | `WATCH` | Open PRs [#14489](https://github.com/OpenHands/OpenHands/pull/14489), [#14579](https://github.com/OpenHands/OpenHands/pull/14579). |
| [OpenHands#14499](https://github.com/OpenHands/OpenHands/issues/14499) | `WATCH` | Open PR [#14540](https://github.com/OpenHands/OpenHands/pull/14540). |
| [trycua#1725](https://github.com/trycua/cua/issues/1725) | `WATCH` | Open PR [#1737](https://github.com/trycua/cua/pull/1737). |
| [crewAI#5930](https://github.com/crewAIInc/crewAI/issues/5930) | `WATCH` | Open PR [#5932](https://github.com/crewAIInc/crewAI/pull/5932). |

## No-go for this pass

| Upstream | Reason |
|---|---|
| [continue#12498](https://github.com/continuedev/continue/issues/12498) | Тонкий Windows/config stack report; нужен minimal repro и latest logs. |
| [opencode#29604](https://github.com/anomalyco/opencode/issues/29604) | Assigned, duplicate bot points to `#13770/#25337`. |
| [opencode#29594](https://github.com/anomalyco/opencode/issues/29594) | Assigned, duplicate bot points to `#27370`; есть adjacent keybind docs issue. |
| [opencode#29599](https://github.com/anomalyco/opencode/issues/29599) | Assigned, Windows desktop crash family needs Windows env. |
