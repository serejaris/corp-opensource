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
| NousResearch/hermes-agent | [#32884](https://github.com/NousResearch/hermes-agent/pull/32884) | [fork branch](https://github.com/serejaris/hermes-agent/tree/fix/codex-null-output-stream) | Закрыт как duplicate после merge через [#32963](https://github.com/NousResearch/hermes-agent/pull/32963) | Проверить релиз, `openai-python#3315`, и hardening follow-up по broad `TypeError` recovery |
| pydantic/pydantic-ai | [#5678](https://github.com/pydantic/pydantic-ai/pull/5678) | [#14](https://github.com/serejaris/corp-opensource/issues/14) | Open, mergeable, CI green; Codex review quota warning only | Watch maintainer/bot review |
| pydantic/pydantic-ai | [#5680](https://github.com/pydantic/pydantic-ai/pull/5680) | [#16](https://github.com/serejaris/corp-opensource/issues/16), [watch](watch/pydantic-ai-5679-textcontent-metadata.md) | Open, mergeable, CI green; Codex review quota warning only | Watch maintainer/bot review |
| pydantic/pydantic-ai | [#5681](https://github.com/pydantic/pydantic-ai/pull/5681) | [watch](watch/pydantic-ai-5671-google-cached-content.md) | Competing PR open, mergeable, CI green | Watch/review only; do not duplicate |
| trycua/cua | [#1725](https://github.com/trycua/cua/issues/1725) | [#15](https://github.com/serejaris/corp-opensource/issues/15), [watch](watch/trycua-1725-windows-click-marker.md) | Likely fixed on main pending Windows smoke | Wait for runner/repro path before PR |
| cline/cline | [#10737](https://github.com/cline/cline/issues/10737) | [#17](https://github.com/serejaris/corp-opensource/issues/17), [watch](watch/cline-10737-mcp-task-progress.md) | Strong bug, but duplicate-race with stale/conflicting PRs | Triage existing PRs; no fresh PR yet |
| openai/codex | [#24704](https://github.com/openai/codex/issues/24704) | [watch](watch/openai-codex-24704-subagent-prompt-cache.md) | Strong subagent/cache bug; reporter has reference PR | Watch; no duplicate PR unless maintainers ask |
| CopilotKit/CopilotKit | [#5035](https://github.com/CopilotKit/CopilotKit/pull/5035) | [#19](https://github.com/serejaris/corp-opensource/issues/19), [watch](watch/copilotkit-4911-workspace-deps.md) | Open, mergeable; docs preview green; other Vercel previews need team authorization | Watch maintainer review |
| langchain-ai/deepagents | [#3616](https://github.com/langchain-ai/deepagents/pull/3616), [#3587](https://github.com/langchain-ai/deepagents/issues/3587) | [#20](https://github.com/serejaris/corp-opensource/issues/20), [watch](watch/deepagents-3587-qwen-tool-call-id.md) | PR auto-closed by assignment guard; approach posted on issue | Wait for maintainer assignment/reopen |
| OpenHands/OpenHands | [#14476](https://github.com/OpenHands/OpenHands/issues/14476) | [#21](https://github.com/serejaris/corp-opensource/issues/21), [watch](watch/openhands-14476-conversation-metadata-race.md) | Candidate/watch; no competing PR found; SaaS semantics risk | Only pursue if unit repro is possible |
| browser-use/browser-use | [#4877](https://github.com/browser-use/browser-use/issues/4877), [#4801](https://github.com/browser-use/browser-use/issues/4801), [#4846](https://github.com/browser-use/browser-use/issues/4846) | [scouting](watch/ai-native-frameworks-scouting-2026-05-27.md) | Good repo, but top candidates already covered by open mergeable PRs | Watch/review only; no duplicate PR |
| OpenHands/software-agent-sdk | [#3394](https://github.com/OpenHands/software-agent-sdk/pull/3394), [#2806](https://github.com/OpenHands/software-agent-sdk/issues/2806) | [#22](https://github.com/serejaris/corp-opensource/issues/22), [watch](watch/openhands-sdk-2806-tmux-viewport.md) | PR open, mergeable, review required; `pr-review` skipped | Watch maintainer review |
| anomalyco/opencode | [#29530](https://github.com/anomalyco/opencode/pull/29530), [#29525](https://github.com/anomalyco/opencode/issues/29525) | [#23](https://github.com/serejaris/corp-opensource/issues/23), [watch](watch/opencode-29525-webfetch-null-format.md) | PR open, mergeable; duplicate/compliance/standards checks green | Watch maintainer review |
| cline/cline | [#11086](https://github.com/cline/cline/issues/11086) | [#24](https://github.com/serejaris/corp-opensource/issues/24), [scouting](watch/ai-native-frameworks-scouting-2026-05-27-cycle-2.md) | Strong signal; no competing PR found; current main did not show reported openai-compatible payload path | Needs exact CLI repro before PR |
| e2b-dev/E2B | [#1352](https://github.com/e2b-dev/E2B/issues/1352) | [#25](https://github.com/serejaris/corp-opensource/issues/25), [scouting](watch/ai-native-frameworks-scouting-2026-05-27-cycle-2.md) | Strategic sandbox reconnect candidate; may need live sandbox | Track; avoid PR until repro/test surface clear |
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

## Правила

- Репозиторий ведём на русском: README, playbooks, watch notes, issue bodies.
- Разведку по новым target repo делаем через субагентов:
  - repo-fit: активность, contribution rules, test surface, maintainer response;
  - bug-signal: свежие баги, дубликаты, реакции, боль пользователей;
  - patchability: где минимальный patch surface и есть targeted tests.
- Не начинаем PR без воспроизведения или понятного failing/regression test.
- Для bugfix PR сначала доказываем, что regression test реально ловит старый баг: временно возвращаем старое поведение или запускаем тест на pre-fix state и записываем точный failing assertion/error.
- Для schema/runtime mismatch regression пишем через raw wire/tool boundary: передаём тот же payload, который видит runtime/модель (`null`, missing default, etc.), а не только typed helper.
- Для publish/release багов проверяем не только helper, но и внешний артефакт/контракт: registry metadata, package manifest, install/runtime symptom или closest publishable fixture.
- Если есть competing PR, ведём duplicate triage публично: что сравнили, что портировали, чего не хватает.
- Duplicate scan повторяем прямо перед кодом. Если candidate уже закрыт open/mergeable PR с тестами, переключаемся в watch/review/validation.
- Реакции и лайки считаем impact-сигналом, но не approval и не CI.

## Playbooks

- [Стратегия open-source contribution](strategy.md)
- [Repo scope: AI-native frameworks and harnesses](watch/repo-scope-ai-native-frameworks.md)
- [Разведка багов в репозиториях](playbooks/repo-bug-scouting.md)
- [Гонка duplicate PR](playbooks/duplicate-pr-race.md)
- [Follow-up по старым upstream PR/issues](playbooks/upstream-followup-loop.md)
- [Runner-контейнер на corp-server](playbooks/runner-container.md)
- [AI-native frameworks scouting cycle 2](watch/ai-native-frameworks-scouting-2026-05-27-cycle-2.md)

## Метки

- `repo-scouting` — поиск и оценка репозиториев.
- `candidate-bug` — потенциальный баг для PR.
- `needs-subagents` — нужна параллельная разведка.
- `needs-repro` — нет подтверждённого воспроизведения.
- `duplicate-triage` — есть конкурирующие PR.
- `release-followup` — проверить, дошёл ли fix до релиза.
