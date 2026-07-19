# Стратегия open-source и developer ecosystem

## Цель

Системно создавать внешнюю техническую ценность двумя способами:

1. находить важные open-source баги, воспроизводить их, чинить маленькими PR с тестами и превращать каждый исход в процессный урок;
2. безопасно выделять reusable artifacts из private/vendor/runtime контекста, публиковать provenance-cleared packages и проверять их на публичной границе.

Обе дорожки питают Developer Ecosystem flywheel:

`real build / user pain → reproducible evidence → upstream fix or public artifact → content/community proof → company/programme relationship → early access/feedback/opportunity → next build`

Operating model и критерии будущего отдельного department: issue #93.

## Дорожка A — upstream contribution

### Какие репозитории подходят

Upfront scope lives in [watch/repo-scope-ai-native-frameworks.md](watch/repo-scope-ai-native-frameworks.md). Перед поиском багов сначала работай из этого списка или явно добавляй новый repo в `candidate / verify` с доказательствами.

Ищем репозитории, где:

- продуктом реально пользуются разработчики, AI builders или infra-команды;
- есть активность за последние 7 дней;
- баги можно воспроизвести локально или через fixture;
- есть понятный test harness;
- maintainers отвечают на issues/PR;
- маленький bugfix PR может быть принят без недельного design process.

Понижаем приоритет:

- спискам, awesome-репам, книгам и roadmaps;
- репам без свежих merged PR;
- огромным корпоративным репам без узкого воспроизводимого бага;
- задачам без тестовой поверхности.

### Основные contribution lanes

| Дорожка | Почему подходит | Примеры |
|---|---|---|
| AI-native company frameworks | Agents, tasks, harnesses, memory, browser/computer-use, evals | `OpenHands/OpenHands`, `cline/cline`, `pydantic/pydantic-ai`, `trycua/cua`, `CopilotKit/CopilotKit` |
| Coding-agent runtimes | Ежедневная боль Hermes/Codex/OpenCode; provider/tool bugs | `NousResearch/hermes-agent`, `anomalyco/opencode`, `cline/cline`, `OpenHands/OpenHands` |
| Harness / eval / sandbox infrastructure | Test harness, replay, fixtures, isolated runners и CI | `trycua/cua`, `browser-use/browser-use`, `e2b-dev/*` |
| AI runtime frameworks | Integration bugs и активные пользователи | `langchain-ai/langchain`, `langflow-ai/langflow`, `pydantic/pydantic-ai` |
| Document conversion and scraping | Хорошо чинится через fixtures и regression tests | `microsoft/markitdown`, `firecrawl/firecrawl` |

Понизить приоритет до нового сигнала:

- `google-gemini/gemini-cli` — возвращать как active lane только при сильном fresh bug/repro signal;
- однодневные star-spike repos без устойчивых issues/maintainer loop — сначала проверять реальную активность.

### Цикл поиска бага

1. Сканировать fresh issues/PR: `bug`, `regression`, `crash`, `TypeError`, `broken`, duplicates, post-release regressions.
2. Оценивать кандидата:
   - local repro `+3`;
   - затрагивает наши workflows `+3`;
   - reactions/duplicates `+2`;
   - small patch + test `+2`;
   - maintainer active за 48 часов `+2`;
   - huge domain context `-3`;
   - no test harness `-2`.
3. Запускать 6 субагентов: repo-fit, bug-signal, repro-path, patchability, duplicate-race, PR-readiness.
4. Открывать candidate-bug issue до реализации.
5. Делать repro в dedicated runner, когда нужна Linux/container среда, тяжёлые зависимости или долгие тесты.
6. Делать 3-subagent critique после parent live gates.
7. Переходить к PR только с доказанным pre-fix fail или точным regression contract.

Canonical playbook: [playbooks/repo-bug-scouting.md](playbooks/repo-bug-scouting.md).

## Дорожка B — public artifact release

### Какие artifacts подходят

- agent skills и plugin bundles;
- prompts, templates, playbooks и starter kits;
- reusable source, выделяемый из private projects;
- course/workshop assets, которые решено открыть;
- benchmark fixtures и datasets;
- demo repos для developer programmes;
- vendor/runtime exports после подтверждения provenance и redistribution rights.

Не публиковать автоматически:

- runtime state, logs, histories, caches, queues;
- credentials, cookies, OAuth state, signing material;
- customer/student/user data;
- private paths, internal links и hostnames;
- vendor-protected или unknown-source assets;
- mixed bundles, которые нельзя безопасно разделить.

### Главный принцип

`inventory before copy; allowlist before package; approval before outward write; fresh public verification before done`

Visibility внутри vendor/runtime продукта не является правом на redistribution. Attribution не заменяет license/permission.

### Release tree

1. Owner epic — зачем и для кого открывается artifact.
2. Inventory/review child — source, version, provenance, license, companion files, classification, allowlist.
3. Publish/verify child — public repo/package только после approved allowlist, затем fresh clone/install/discovery smoke.

Evaluation, infrastructure, content launch и commercial work остаются отдельными owner scopes.

### State machine

`DISCOVERED → INVENTORY → PROVENANCE-CLEARED → LICENSE-CLEARED → SECURITY-CLEARED → PACKAGE-READY → APPROVED → PUBLISHED → VERIFIED → MAINTAINED`

Block states:

- `BLOCKED-PROVENANCE`
- `BLOCKED-LICENSE`
- `BLOCKED-SECURITY`
- `BLOCKED-OWNER-APPROVAL`
- `NO-GO-VENDOR-PROTECTED`
- `NO-GO-MIXED-PRIVATE-DATA`

### Release cycle

1. Capture release intent, target, audience and owners.
2. Create `release-manifest.yaml` before copying files.
3. Classify every file and complete bundle.
4. Prove source/version/ownership and redistribution basis.
5. Review licenses, notices, terms and intended public license compatibility.
6. Create a clean staging tree from explicit allowlist.
7. Run manual classification plus secret/private-path scans on the staged tree.
8. Add README, install/use, license, attribution, security contact, limitations and maintenance owner.
9. Show dry run and receive explicit approval for the exact public target/artifact set.
10. Publish from clean staging.
11. Fresh-clone the public artifact and run documented install/discovery/smoke plus repeated scans.
12. Record release evidence and update the reusable skill with new lessons.

Canonical playbook: [playbooks/safe-public-release.md](playbooks/safe-public-release.md).

Reusable public skill: `serejaris/personal-corp-skills/skills/safe-public-release`.

Source implementation tracker: issue #92. First project: Kimi skills issues #88–90.

## Runner-контур

Целевой runtime: `corp-opensource-runner` на `corp-server`.

- `corp-opensource` отвечает за scouting, issue queue, watch notes, PR decisions и release assurance.
- `corp-server` отвечает за CT/VM provisioning, resources, lifecycle, backups и access policy.
- CT `216` / `pc-hermes-test` остаётся Hermes-specific smoke/repro container.
- Infra mutation проходит через `corp-server-ops`, issue/runbook и approval policy.
- Secrets находятся только в environment/host-managed paths.
- Runner не открывает upstream PR и не публикует public artifact автоматически.

## Cross-repo routing

| Работа | Owner |
|---|---|
| Upstream scouting/repro/PR gate | `corp-opensource` |
| Public release provenance/license/security/package | `corp-opensource` |
| Product/tool evaluation and maintained source | `corp-dev` / product owner |
| Programme/company research | `corp-research` |
| Creator content, distribution and relationships | `corp-youtube` |
| Commercial negotiation after interest | CRM / sales owner |
| Policy, exclusivity and public approval | HQ / founder |
| Runner and scan infrastructure | `corp-server` |

## Ритм

- Раз в неделю: обновить candidate repos через live GitHub search.
- Раз в неделю: проверить Paperclip-like harness/framework repos.
- Ежедневно: проверить active upstream watch issues и PR follow-up.
- При competing PR: duplicate triage.
- При новом candidate public artifact: создать owner release tree до extraction.
- После каждого PR/release outcome: обновить watch note/playbook/skill.
- Раз в 4 недели: review Developer Ecosystem Office split criteria из issue #93.

## Метрики

### Contribution

- candidate bugs;
- reproduced bugs;
- comments/PRs opened;
- merged PRs or landed upstream fixes;
- maintainer interactions;
- lessons promoted into skills.

### Public release

- artifacts inventoried;
- packages blocked by provenance/license/security;
- packages reaching `PACKAGE-READY`;
- public releases;
- fresh-clone verified releases;
- installs/reuse/references when available;
- maintenance reviews completed;
- lessons promoted into `safe-public-release`.

### Developer ecosystem

- programme applications and acceptances;
- external technical relationships;
- events/workshops/community activities;
- early-access/credits/feedback opportunities;
- commercial handoffs created from confirmed external interest;
- competing-programme conflict decisions.
