# mcp-use/mcp-use scouting, 2026-05-27

Internal tracker: [#57](https://github.com/serejaris/corp-opensource/issues/57). Umbrella: [#52](https://github.com/serejaris/corp-opensource/issues/52).

Final `next_status`: `WATCH`.

## Итог

`mcp-use/mcp-use` остаётся сильным AI-native target: MCP app/client/server integration framework, около 10k stars, активный TypeScript/Python monorepo, MIT, свежий push 2026-05-27. Но issue-level scan оказался duplicate-heavy: почти все узкие lanes уже закрыты активными или merged upstream PR.

Upstream PR/comment не делались.

## Process gates

- 6-subagent scouting: выполнен.
- Обязательные skills `open-source-bug-scouting` / `open-source-pr-workflow`: недоступны в текущем окружении; использован documented fallback через live GitHub/`gh`, shallow checkout и multi-agent triage.
- Parent live gates: repo state, issue/PR state, assignee/labels, linked PRs/timeline, PR search, contribution docs, toolchain/test gates проверены.
- 3-subagent critique: фактология/дубликаты, process gates, actionability выполнены; все роли подтвердили `WATCH`.
- Runner repro: не запускался, потому что нет PR-ready lane. Это блокер для `PR-READY`, но не для watch-only решения.
- Upstream action: 0 PR, 0 comment.

## Parent live gates

- Repo: `mcp-use/mcp-use`.
- Default branch: `main`.
- License: MIT.
- Latest release: `python-v1.7.0`, 2026-03-17.
- Live issue/PR count at scan: 54 open issues, 42 open PRs.
- Contribution docs: `CONTRIBUTING.md`, `SECURITY.md`, PR template present.
- Security note: public security-adjacent comments require separate gate; private security path is GitHub Security Advisory or `security@manufact.com`.
- Toolchain:
  - Python: `libraries/python`, Python `>=3.11`, `uv`, `ruff check .`, `ruff format --check .`, `pytest tests/unit`.
  - TypeScript: `libraries/typescript`, pnpm workspace, Node 20+/22 recommended, `pnpm format:check`, `pnpm lint`, `pnpm build`, `pnpm test`, changeset for publishable/user-facing changes.
- Base branch caution: contribution docs say branch from `main`, but active work also flows through `canary`; verify target branch per PR before any future patch.

## Duplicate map

| Lane | Status | Evidence |
|---|---|---|
| Browser entry imports optional LangChain peers | `NO-GO / WATCH` | `#1581` covered by open `#1582`. |
| OAuth RFC 8414 path-suffix metadata | `NO-GO / WATCH` | `#1576` assigned and covered by open `#1578`. |
| OAuth token/error redaction | `NO-GO / WATCH` | `#1570` covered by open `#1573`; security-adjacent lane. |
| FileSystemSessionStore write throttling | `NO-GO / WATCH` | `#1568` covered by open `#1574`. |
| FileSystemSessionStore path traversal | `NO-GO / WATCH` | `#1569` assigned and covered by open `#1572`; security-adjacent lane. |
| Inspector `0.0.0.0` autoConnect URL | `NO-GO / WATCH` | `#1551` covered by merged `#1552` on 2026-05-24; issue still open but lane is not free. |
| Inspector copy button for user messages | `NO-GO / WATCH` | `#1555` covered by merged `#1556` on 2026-05-27; issue still open but lane is not free. |
| Widget Vite base / production static routes | `WATCH` | `#1553` covered/overlapped by merged `#1560` plus broad open `#1531` basePath work. |
| Python router `include_router(prefix=...)` resources | `NO-GO / WATCH` | `#1438` covered by open `#1439` and `#1455`. |
| Python WebSocket/Sandbox disconnect double-close | `NO-GO / WATCH` | `#1415` covered primarily by open `#1421`; related open chain `#1226/#1412` remains relevant. |

## Critique synthesis

Factology/duplicates: подтвердил, что прямые patch lanes закрыты active/merged PR. Для `#1551`, `#1555`, `#1553` нельзя считать lane свободной только потому, что issue открыт; есть явный merged или overlapping PR.

Process gates: `WATCH` корректен, если tracker фиксирует fallback, parent gates, duplicate map, отсутствие runner repro из-за отсутствия PR-ready lane, и отсутствие upstream action.

Actionability: `COMMENT-FIRST` сейчас не нужен, потому что upstream уже имеет явные PR-связки или maintainer-owned lanes. Новый комментарий добавит шум и не разблокирует действие.

## Watch triggers

Пересмотреть статус только если:

- covering PR закрыт без merge и issue остаётся open;
- maintainer просит alternate implementation или tests;
- issue reopened или остаётся воспроизводимым на current target branch после merged fix;
- CI/review блокирует covering PR на небольшой недостающей проверке, которую можно воспроизвести в runner;
- lane теряет assignee и остаётся inactive достаточно долго для fresh live gate.

До такого события: watch/review only, no upstream PR/comment.
