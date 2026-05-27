# Corp Open Source

Приватный контур для системной работы с open-source: какие upstream-репозитории смотрим, какие баги проверяем, какие PR ведём, где есть конкурирующие PR и какие уроки надо переносить в skills.

## Execution model

- Разведка и triage идут через 6 субагентов, если есть несколько repo/PR или тема быстро меняется.
- Repro, targeted tests и тяжёлые checkout должны уходить в dedicated runner на `corp-server`: `corp-opensource-runner`.
- Пока runner не создан, CT `216` / `pc-hermes-test` можно использовать только для Hermes-specific bounded smoke/repro checks. Это не общий runner.
- Provisioning runner-контейнера живёт в `corp-server`; этот repo хранит очередь, watch notes и решения по upstream.

## Активный watchlist

| Проект | Upstream | Наш контур | Статус | Следующее действие |
|---|---:|---:|---|---|
| NousResearch/hermes-agent | [#32999](https://github.com/NousResearch/hermes-agent/pull/32999), [#32963](https://github.com/NousResearch/hermes-agent/pull/32963), [#33042](https://github.com/NousResearch/hermes-agent/pull/33042) | [#6](https://github.com/serejaris/corp-opensource/issues/6), [watch](watch/hermes-codex-null-output.md) | #32963 merged outage fix; #32999 closed obsolete after #33042 removed the SDK stream helper path entirely | Accept closure; keep boundary-regression lesson in skill/watch |
| pydantic/pydantic-ai | [#5678](https://github.com/pydantic/pydantic-ai/pull/5678) | [#14](https://github.com/serejaris/corp-opensource/issues/14) | Open, mergeable, CI green; Codex review quota warning only | Watch maintainer/bot review |
| pydantic/pydantic-ai | [#5680](https://github.com/pydantic/pydantic-ai/pull/5680) | [#16](https://github.com/serejaris/corp-opensource/issues/16), [watch](watch/pydantic-ai-5679-textcontent-metadata.md) | Open, mergeable, CI green; Codex review quota warning only | Watch maintainer/bot review |
| pydantic/pydantic-ai | [#5688](https://github.com/pydantic/pydantic-ai/issues/5688) | [#47](https://github.com/serejaris/corp-opensource/issues/47), [cycle 15](watch/ai-native-frameworks-scouting-2026-05-27-cycle-15.md), [patch prep](watch/pydantic-ai-5688-mcp-http-client-follow-redirects.md) | Local regression-first patch prepared; comment-first because pydanty says PR opener is delegated and repo has assignment/maintainer gate | Wait assignment/confirmation or pydanty PR; no racing PR |
| pydantic/pydantic-ai | [#5681](https://github.com/pydantic/pydantic-ai/pull/5681) | [watch](watch/pydantic-ai-5671-google-cached-content.md) | Competing PR open, mergeable, CI green | Watch/review only; do not duplicate |
| trycua/cua | [#1725](https://github.com/trycua/cua/issues/1725) | [#15](https://github.com/serejaris/corp-opensource/issues/15), [watch](watch/trycua-1725-windows-click-marker.md) | Likely fixed on main pending Windows smoke | Wait for runner/repro path before PR |
| cline/cline | [#10737](https://github.com/cline/cline/issues/10737) | [#17](https://github.com/serejaris/corp-opensource/issues/17), [watch](watch/cline-10737-mcp-task-progress.md) | Strong bug, but duplicate-race with stale/conflicting PRs | Triage existing PRs; no fresh PR yet |
| openai/codex | [#24704](https://github.com/openai/codex/issues/24704) | [watch](watch/openai-codex-24704-subagent-prompt-cache.md) | Strong subagent/cache bug; reporter has reference PR | Watch; no duplicate PR unless maintainers ask |
| CopilotKit/CopilotKit | [#5035](https://github.com/CopilotKit/CopilotKit/pull/5035) | [#19](https://github.com/serejaris/corp-opensource/issues/19), [watch](watch/copilotkit-4911-workspace-deps.md) | Open, mergeable; docs preview green; other Vercel previews need team authorization | Watch maintainer review |
| langchain-ai/deepagents | [#3616](https://github.com/langchain-ai/deepagents/pull/3616), [#3587](https://github.com/langchain-ai/deepagents/issues/3587) | [#20](https://github.com/serejaris/corp-opensource/issues/20), [watch](watch/deepagents-3587-qwen-tool-call-id.md) | PR auto-closed by assignment guard; approach posted on issue | Wait for maintainer assignment/reopen |
| OpenHands/OpenHands | [#14563](https://github.com/OpenHands/OpenHands/issues/14563) | [#26](https://github.com/serejaris/corp-opensource/issues/26) | Watch / maintainer-direction-needed; container visibility vs personal skills roadmap | Wait maintainer direction |
| browser-use/browser-use | [#4877](https://github.com/browser-use/browser-use/issues/4877), [#4801](https://github.com/browser-use/browser-use/issues/4801), [#4846](https://github.com/browser-use/browser-use/issues/4846) | [scouting](watch/ai-native-frameworks-scouting-2026-05-27.md) | Good repo, but top candidates already covered by open mergeable PRs | Watch/review only; no duplicate PR |
| browser-use/browser-use | [#4796](https://github.com/browser-use/browser-use/issues/4796), [#4815](https://github.com/browser-use/browser-use/pull/4815) | [#27](https://github.com/serejaris/corp-opensource/issues/27) | Duplicate-covered; PR open/mergeable/green in tracked state | Watch/review only |
| OpenHands/software-agent-sdk | [#3394](https://github.com/OpenHands/software-agent-sdk/pull/3394), [#2806](https://github.com/OpenHands/software-agent-sdk/issues/2806) | [#22](https://github.com/serejaris/corp-opensource/issues/22), [watch](watch/openhands-sdk-2806-tmux-viewport.md) | PR open; maintainer said it looks fine but wants eval; fork workflows on `3c8eae78` are still `action_required` | Wait maintainer eval / workflow approval |
| anomalyco/opencode | [#29530](https://github.com/anomalyco/opencode/pull/29530), [#29525](https://github.com/anomalyco/opencode/issues/29525) | [#23](https://github.com/serejaris/corp-opensource/issues/23), [watch](watch/opencode-29525-webfetch-null-format.md) | PR open, mergeable; duplicate/compliance/standards checks green | Watch maintainer review |
| cline/cline | [#11087](https://github.com/cline/cline/pull/11087), [#11086](https://github.com/cline/cline/issues/11086) | [#24](https://github.com/serejaris/corp-opensource/issues/24), [scouting](watch/ai-native-frameworks-scouting-2026-05-27-cycle-7.md) | PR open, mergeable; Greptile gap addressed; Quality Checks + Ubuntu/Windows SDK tests green | Watch maintainer review |
| cline/cline | [#10139](https://github.com/cline/cline/issues/10139), [#10384](https://github.com/cline/cline/pull/10384), [#10140](https://github.com/cline/cline/pull/10140), [#10141](https://github.com/cline/cline/pull/10141) | [cycle 10](watch/ai-native-frameworks-scouting-2026-05-27-cycle-10.md) | Duplicate-race; #10384 has strongest configurable fix/tests but all competing PRs conflict | Watch maintainer consolidation; no PR |
| e2b-dev/E2B | [#1352](https://github.com/e2b-dev/E2B/issues/1352) | [#25](https://github.com/serejaris/corp-opensource/issues/25), [scouting](watch/ai-native-frameworks-scouting-2026-05-27-cycle-2.md) | Strategic sandbox reconnect candidate; may need live sandbox | Track; avoid PR until repro/test surface clear |
| langchain-ai/langgraph | [#7688](https://github.com/langchain-ai/langgraph/issues/7688) | [#48](https://github.com/serejaris/corp-opensource/issues/48), [watch](watch/langgraph-7688-timewait-port.md), [cycle 17](watch/ai-native-frameworks-scouting-2026-05-27-cycle-17.md) | WATCH / reproduced: installed `langgraph-api==0.8.0` misclassifies `TIME_WAIT` as active port use; [evidence comment posted](https://github.com/langchain-ai/langgraph/issues/7688#issuecomment-4554205543) | Wait maintainer direction on public `langgraph-api` patch surface before PR |
| aaif-goose/goose | [#9136](https://github.com/aaif-goose/goose/issues/9136) | [#28](https://github.com/serejaris/corp-opensource/issues/28) | Real bug; no duplicate PR; comment-first assignment ask posted | Wait maintainer assignment |
| pydantic/pydantic-ai | [#5419](https://github.com/pydantic/pydantic-ai/issues/5419), [#5443](https://github.com/pydantic/pydantic-ai/pull/5443) | [#29](https://github.com/serejaris/corp-opensource/issues/29) | Duplicate-covered; PR open/mergeable but coverage/check failing in tracked state | Watch existing PR, no duplicate |
| agno-agi/agno | [#8062](https://github.com/agno-agi/agno/issues/8062), [#8124](https://github.com/agno-agi/agno/pull/8124) | [#30](https://github.com/serejaris/corp-opensource/issues/30), [cycle 8](watch/ai-native-frameworks-scouting-2026-05-27-cycle-8.md) | Duplicate-covered; our repro/patch worked, but #8124 auto-closed because #8065/#8084 already referenced #8062 | Watch #8065/#8084; offer integration-style MCP tool test if maintainers ask |
| langchain-ai/langgraph | [#7684](https://github.com/langchain-ai/langgraph/issues/7684) | [#31](https://github.com/serejaris/corp-opensource/issues/31), [cycle 8](watch/ai-native-frameworks-scouting-2026-05-27-cycle-8.md) | No-go; three assignment-gated PRs already closed | Watch only |
| trycua/cua | [#1724](https://github.com/trycua/cua/issues/1724) | [#32](https://github.com/serejaris/corp-opensource/issues/32), [cycle 8](watch/ai-native-frameworks-scouting-2026-05-27-cycle-8.md) | Comment-first; direct stdio launch support needs confirmation | Repro/comment only with new evidence |
| openai/codex | [#24612](https://github.com/openai/codex/issues/24612) | [#33](https://github.com/serejaris/corp-opensource/issues/33), [cycle 8](watch/ai-native-frameworks-scouting-2026-05-27-cycle-8.md) | Candidate/watch; no direct PR found; provider-history sanitizer bug | Watch/access-sensitive |
| aaif-goose/goose | [#9332](https://github.com/aaif-goose/goose/issues/9332) | [#34](https://github.com/serejaris/corp-opensource/issues/34), [watch](watch/goose-9332-pdeathsig-mcp-subprocess.md) | Comment-first; no duplicate PR; maintainer-alignment comment posted | Wait direction or run Linux repro on runner |
| aaif-goose/goose | [#9446](https://github.com/aaif-goose/goose/issues/9446) | [#49](https://github.com/serejaris/corp-opensource/issues/49), [cycle 18](watch/ai-native-frameworks-scouting-2026-05-27-cycle-18.md), [patch prep](watch/goose-9446-response-completed-missing-output.md) | Local regression-first patch prepared; no duplicate PR found in tracked state | Run final PR-readiness gate before upstream PR |
| BerriAI/litellm | [#28962](https://github.com/BerriAI/litellm/issues/28962) | [#35](https://github.com/serejaris/corp-opensource/issues/35), [cycle 9](watch/ai-native-frameworks-scouting-2026-05-27-cycle-9.md) | No-go / release-followup; mocked current branch maps Gemini 503 `MaskedHTTPStatusError` to `ServiceUnavailableError` | Watch reporter/upstream; no PR unless exact leaking path is confirmed |
| microsoft/playwright-mcp | [#1635](https://github.com/microsoft/playwright-mcp/issues/1635) | [#36](https://github.com/serejaris/corp-opensource/issues/36), [cycle 9](watch/ai-native-frameworks-scouting-2026-05-27-cycle-9.md) | Comment-first / assignment-gated; [regression offer posted](https://github.com/microsoft/playwright-mcp/issues/1635#issuecomment-4552993898) | Wait maintainer direction; no PR without approval |
| modelcontextprotocol/typescript-sdk | [#2155](https://github.com/modelcontextprotocol/typescript-sdk/issues/2155), [#1925](https://github.com/modelcontextprotocol/typescript-sdk/pull/1925), [#1926](https://github.com/modelcontextprotocol/typescript-sdk/pull/1926) | [#37](https://github.com/serejaris/corp-opensource/issues/37), [cycle 9](watch/ai-native-frameworks-scouting-2026-05-27-cycle-9.md) | Duplicate-triage; server-side SSE escaping already covered by open mergeable PRs with tests | Watch maintainer review; validate only uncovered client fail-fast / JSON-response gap |
| modelcontextprotocol/typescript-sdk | [#2108](https://github.com/modelcontextprotocol/typescript-sdk/issues/2108), [#2111](https://github.com/modelcontextprotocol/typescript-sdk/pull/2111) | [cycle 10](watch/ai-native-frameworks-scouting-2026-05-27-cycle-10.md) | Duplicate-covered; #2111 covers both mismatch directions plus match/absent initialize cases with green checks | Watch maintainer review; no PR |
| modelcontextprotocol/typescript-sdk | [#2126](https://github.com/modelcontextprotocol/typescript-sdk/issues/2126), [#2140](https://github.com/modelcontextprotocol/typescript-sdk/pull/2140), [#2127](https://github.com/modelcontextprotocol/typescript-sdk/pull/2127) | [cycle 13](watch/ai-native-frameworks-scouting-2026-05-27-cycle-13.md) | Duplicate-covered by #2140; #2127 closed duplicate; #2140 has regression + changeset but one unrelated-looking integration job red | Watch #2140; no PR |
| modelcontextprotocol/python-sdk | [#2687](https://github.com/modelcontextprotocol/python-sdk/issues/2687) | [#38](https://github.com/serejaris/corp-opensource/issues/38), [cycle 9](watch/ai-native-frameworks-scouting-2026-05-27-cycle-9.md) | Comment-first / maintainer-gated; pre-fix `AnyHttpUrl` vs `AnyUrl` redirect mismatch reproduced and [PR offer posted](https://github.com/modelcontextprotocol/python-sdk/issues/2687#issuecomment-4553121932) | Wait maintainer confirmation/assignment before PR |
| google/adk-python | [#5864](https://github.com/google/adk-python/issues/5864) | [#46](https://github.com/serejaris/corp-opensource/issues/46), [cycle 14](watch/ai-native-frameworks-scouting-2026-05-27-cycle-14.md), [patch prep](watch/google-adk-5864-vertexai-import-guard.md) | Local regression-first patch prepared; issue still assigned/`request clarification`, maintainer asked reporter to validate full test pass; no duplicate PR found | Wait maintainer/reporter direction before PR |
| modelcontextprotocol/python-sdk | [#2578](https://github.com/modelcontextprotocol/python-sdk/issues/2578), [#2590](https://github.com/modelcontextprotocol/python-sdk/pull/2590), [#2645](https://github.com/modelcontextprotocol/python-sdk/pull/2645), [#2646](https://github.com/modelcontextprotocol/python-sdk/pull/2646) | [cycle 10](watch/ai-native-frameworks-scouting-2026-05-27-cycle-10.md) | Duplicate-covered; all three linked PRs cover refresh resource omission + trailing slash normalization with green checks | Watch maintainer selection; no PR |
| BerriAI/litellm | [#28971](https://github.com/BerriAI/litellm/issues/28971), [#28970](https://github.com/BerriAI/litellm/pull/28970) | [#39](https://github.com/serejaris/corp-opensource/issues/39), [cycle 10](watch/ai-native-frameworks-scouting-2026-05-27-cycle-10.md) | Comment-first / missing-regression-offered; #28970 is adjacent `/apply_guardrail`, not exact `/v1/responses` spend-log path | Wait maintainer signal before PR |
| e2b-dev/E2B | [#1349](https://github.com/e2b-dev/E2B/issues/1349), [#1354](https://github.com/e2b-dev/E2B/pull/1354), [#1355](https://github.com/e2b-dev/E2B/pull/1355) | [#39](https://github.com/serejaris/corp-opensource/issues/39), [cycle 10](watch/ai-native-frameworks-scouting-2026-05-27-cycle-10.md), [cycle 18](watch/ai-native-frameworks-scouting-2026-05-27-cycle-18.md) | #1354 closed as superseded by maintainer/superset #1355; #1355 covers JS + Python SDK and has CLA/Vercel green, SDK checks still running in tracked state | Watch #1355; no further action on #1354 |
| openai/codex | [#24725](https://github.com/openai/codex/issues/24725) | [#41](https://github.com/serejaris/corp-opensource/issues/41), [cycle 11](watch/ai-native-frameworks-scouting-2026-05-27-cycle-11.md) | Regression-first candidate, but upstream external PRs are invitation-only | Watch/comment-first; no PR without maintainer signal |
| anomalyco/opencode | [#15226](https://github.com/anomalyco/opencode/issues/15226), [#29565](https://github.com/anomalyco/opencode/pull/29565) | [#44](https://github.com/serejaris/corp-opensource/issues/44), [watch](watch/opencode-15226-structured-output-thinking.md), [cycle 12](watch/ai-native-frameworks-scouting-2026-05-27-cycle-12.md) | PR open, ready, mergeable; standards/compliance/duplicate checks green | Watch maintainer review; no extra opencode PR until review or distinct gap |
| langchain-ai/deepagents | [#3568](https://github.com/langchain-ai/deepagents/issues/3568) | [scouting](watch/ai-native-frameworks-scouting-2026-05-27-cycle-2.md) | Small prompt/schema bug, but reporter has local fix and assignment gate risk | Watch/comment only if maintainers ask |

## Рабочие issues

- [#1 Track Hermes Codex null-output upstream fix](https://github.com/serejaris/corp-opensource/issues/1)
- [#2 Launch open-source bug contribution program](https://github.com/serejaris/corp-opensource/issues/2)
- [#3 Scout Hermes follow-up bugs after Codex null-output merge](https://github.com/serejaris/corp-opensource/issues/3)
- [#4 Scout opencode for reproducible coding-agent bugs](https://github.com/serejaris/corp-opensource/issues/4)
- [#5 Scout MarkItDown for fixture-friendly conversion bugs](https://github.com/serejaris/corp-opensource/issues/5)
- [#6 Hermes Codex recovery may mask unrelated TypeError after #32963](https://github.com/serejaris/corp-opensource/issues/6)
- [#7 MarkItDown IpynbConverter crashes on non-ASCII PDF](https://github.com/serejaris/corp-opensource/issues/7)
- [#8 opencode Task subagents omit tools for Bedrock/LiteLLM](https://github.com/serejaris/corp-opensource/issues/8)
- [#9 Firecrawl cancel crawl floods webhook failures](https://github.com/serejaris/corp-opensource/issues/9)
- [#10 Provision corp-opensource runner container on corp-server](https://github.com/serejaris/corp-opensource/issues/10)
- [#11 Weekly AI-native framework top scan](https://github.com/serejaris/corp-opensource/issues/11)
- [#12 Design Hermes/Codex worker pool for parallel scouting](https://github.com/serejaris/corp-opensource/issues/12)
- [#13 Scout Paperclip-like harness expansion opportunities](https://github.com/serejaris/corp-opensource/issues/13)
- [#14 pydantic-ai serialize_any loses binary data](https://github.com/serejaris/corp-opensource/issues/14)
- [#15 trycua Windows element-index clicks miss click.png marker](https://github.com/serejaris/corp-opensource/issues/15)
- [#16 pydantic-ai TextContent metadata lost in UI adapter round-trip](https://github.com/serejaris/corp-opensource/issues/16)
- [#17 cline MCP task_progress leaks into MCP args](https://github.com/serejaris/corp-opensource/issues/17)
- [#18 openai/codex forked subagents lose prompt-cache lineage](https://github.com/serejaris/corp-opensource/issues/18)
- [#19 CopilotKit workspace protocol deps leak into published package](https://github.com/serejaris/corp-opensource/issues/19)
- [#20 deepagents subagent task fails without tool_call_id on OpenAI-compatible Qwen](https://github.com/serejaris/corp-opensource/issues/20)
- [#21 OpenHands conversation metadata race hides started resolver conversations](https://github.com/serejaris/corp-opensource/issues/21)
- [#22 OpenHands SDK tmux 1000x1000 viewport burns CPU](https://github.com/serejaris/corp-opensource/issues/22)
- [#23 opencode webfetch nullable format mismatch](https://github.com/serejaris/corp-opensource/issues/23)
- [#24 cline CLI sends unsupported reasoning field to OpenAI-compatible GLM endpoints](https://github.com/serejaris/corp-opensource/issues/24)
- [#25 E2B resumed sandbox stdout stream wedges after large response](https://github.com/serejaris/corp-opensource/issues/25)
- [#26 OpenHands global skills не видны в openhands serve](https://github.com/serejaris/corp-opensource/issues/26)
- [#27 browser-use boolean index action schema](https://github.com/serejaris/corp-opensource/issues/27)
- [#28 goose repeated tool calls bypass RepetitionInspector](https://github.com/serejaris/corp-opensource/issues/28)
- [#29 pydantic-ai multi-MCP discovery AG-UI stalls](https://github.com/serejaris/corp-opensource/issues/29)
- [#30 agno MCP tools crash when stream=True returns async iterator](https://github.com/serejaris/corp-opensource/issues/30)
- [#31 LangGraph PostgresStore numeric filters assignment-gated](https://github.com/serejaris/corp-opensource/issues/31)
- [#32 trycua cua-driver mcp SIGABRT before handshake](https://github.com/serejaris/corp-opensource/issues/32)
- [#33 Codex web_search_call history crashes custom provider after model switch](https://github.com/serejaris/corp-opensource/issues/33)
- [#34 goose PR_SET_PDEATHSIG kills stdio MCP subprocesses](https://github.com/serejaris/corp-opensource/issues/34)
- [#35 LiteLLM Gemini AI Studio 5xx error mapping](https://github.com/serejaris/corp-opensource/issues/35)
- [#36 Playwright MCP browser_navigate_back CDP timeout](https://github.com/serejaris/corp-opensource/issues/36)
- [#37 MCP TypeScript SDK U+2028/U+2029 SSE timeout duplicate triage](https://github.com/serejaris/corp-opensource/issues/37)
- [#38 MCP Python SDK AnyUrl/AnyHttpUrl OAuth redirect mismatch](https://github.com/serejaris/corp-opensource/issues/38)
- [#39 Cycle 10 scouting: LiteLLM guardrail duplicate gate and E2B next repro](https://github.com/serejaris/corp-opensource/issues/39)
- [#40 Cycle 11 scouting: regression-first AI-native bug targets](https://github.com/serejaris/corp-opensource/issues/40)
- [#41 Codex plugin-namespaced skills exceed MAX_NAME_LEN](https://github.com/serejaris/corp-opensource/issues/41)
- [#42 Cycle 12 scouting: unblock next AI-native contribution target](https://github.com/serejaris/corp-opensource/issues/42)
- [#44 opencode structured output conflicts with thinking tool_choice](https://github.com/serejaris/corp-opensource/issues/44)
- [#45 Cycle 13 scouting: no safe new PR after six-agent triage](https://github.com/serejaris/corp-opensource/issues/45)
- [#46 google-adk vertexai optional dependency import guard](https://github.com/serejaris/corp-opensource/issues/46)
- [#47 pydantic-ai MCPToolset http_client follow_redirects guard](https://github.com/serejaris/corp-opensource/issues/47)
- [#48 LangGraph dev TIME_WAIT port false-positive](https://github.com/serejaris/corp-opensource/issues/48)

## Правила

- Репозиторий ведём на русском: README, playbooks, watch notes, issue bodies.
- Для bugfix PR сначала оформляем regression card в issue/watch note: `контракт`, `test file`, `command`, `pre-fix fail`, `post-fix pass`. Если pre-fix failure нельзя дешево доказать, candidate остаётся `WATCH / needs-repro`, а не ready PR.
- Разведку по новым target repo делаем через субагентов:
  - repo-fit: активность, contribution rules, test surface, maintainer response;
  - bug-signal: свежие баги, дубликаты, реакции, боль пользователей;
  - patchability: где минимальный patch surface и есть targeted tests.
- Не начинаем PR без воспроизведения или понятного failing/regression test.
- Для bugfix PR сначала доказываем, что regression test реально ловит старый баг: временно возвращаем старое поведение или запускаем тест на pre-fix state и записываем точный failing assertion/error.
- Для schema/runtime mismatch regression пишем через raw wire/tool boundary: передаём тот же payload, который видит runtime/модель (`null`, missing default, etc.), а не только typed helper.
- Для downstream recovery вокруг SDK/dependency parser failures тест обязан доказать две стороны границы: SDK failure восстанавливается через публичный runtime path, а app-side callback/handler `TypeError` с таким же текстом не проглатывается. Helper predicate test здесь недостаточен.
- Для publish/release багов проверяем не только helper, но и внешний артефакт/контракт: registry metadata, package manifest, install/runtime symptom или closest publishable fixture.
- Для optional dependency import bugs regression должен проверять публичную границу импорта и понятный missing-extra/install-extra error. Helper-only тест недостаточен, если пользовательский симптом — raw `ModuleNotFoundError` при импорте.
- Для adapter bugs вокруг user-supplied client/callback/factory regression должен идти через публичный constructor/config API и runtime callback shape, которую реально вызывает зависимость. Helper-only тест недостаточен.
- Если есть competing PR, ведём duplicate triage публично: что сравнили, что портировали, чего не хватает.
- Duplicate scan повторяем прямо перед кодом. Если candidate уже закрыт open/mergeable PR с тестами, переключаемся в watch/review/validation.
- Реакции и лайки считаем impact-сигналом, но не approval и не CI.

## Playbooks

- [Стратегия open-source contribution](strategy.md)
- [Repo scope: AI-native frameworks and harnesses](watch/repo-scope-ai-native-frameworks.md)
- [Разведка багов в репозиториях](playbooks/repo-bug-scouting.md)
- [Гонка duplicate PR](playbooks/duplicate-pr-race.md)
- [Follow-up по старым upstream PR/issues](playbooks/upstream-followup-loop.md)
- [Upstream follow-up 2026-05-27](watch/upstream-followup-2026-05-27.md)
- [Runner-контейнер на corp-server](playbooks/runner-container.md)
- [AI-native frameworks scouting cycle 2](watch/ai-native-frameworks-scouting-2026-05-27-cycle-2.md)
- [AI-native frameworks scouting cycle 7](watch/ai-native-frameworks-scouting-2026-05-27-cycle-7.md)
- [AI-native frameworks scouting cycle 8](watch/ai-native-frameworks-scouting-2026-05-27-cycle-8.md)
- [AI-native frameworks scouting cycle 9](watch/ai-native-frameworks-scouting-2026-05-27-cycle-9.md)
- [AI-native frameworks scouting cycle 10](watch/ai-native-frameworks-scouting-2026-05-27-cycle-10.md)
- [AI-native frameworks scouting cycle 11](watch/ai-native-frameworks-scouting-2026-05-27-cycle-11.md)
- [AI-native frameworks scouting cycle 12](watch/ai-native-frameworks-scouting-2026-05-27-cycle-12.md)
- [AI-native frameworks scouting cycle 12 PR-readiness](watch/ai-native-frameworks-scouting-2026-05-27-cycle-12-pr-readiness.md)
- [AI-native frameworks scouting cycle 13](watch/ai-native-frameworks-scouting-2026-05-27-cycle-13.md)
- [AI-native frameworks scouting cycle 14](watch/ai-native-frameworks-scouting-2026-05-27-cycle-14.md)
- [AI-native frameworks scouting cycle 15](watch/ai-native-frameworks-scouting-2026-05-27-cycle-15.md)
- [AI-native frameworks scouting cycle 16 duplicate gate](watch/ai-native-frameworks-scouting-2026-05-27-cycle-16.md)
- [AI-native frameworks scouting cycle 17](watch/ai-native-frameworks-scouting-2026-05-27-cycle-17.md)
- [AI-native frameworks scouting cycle 18](watch/ai-native-frameworks-scouting-2026-05-27-cycle-18.md)
- [google/adk-python #5864 Vertex AI optional dependency import guard](watch/google-adk-5864-vertexai-import-guard.md)
- [pydantic-ai #5688 MCPToolset http_client follow_redirects patch prep](watch/pydantic-ai-5688-mcp-http-client-follow-redirects.md)
- [LangGraph #7688 TIME_WAIT port false-positive](watch/langgraph-7688-timewait-port.md)
- [Goose #9332 PDEATHSIG MCP subprocess lifecycle](watch/goose-9332-pdeathsig-mcp-subprocess.md)
- [Goose #9446 response.completed missing output](watch/goose-9446-response-completed-missing-output.md)

## Метки

- `repo-scouting` — поиск и оценка репозиториев.
- `candidate-bug` — потенциальный баг для PR.
- `needs-subagents` — нужна параллельная разведка.
- `needs-repro` — нет подтверждённого воспроизведения.
- `duplicate-triage` — есть конкурирующие PR.
- `release-followup` — проверить, дошёл ли fix до релиза.
