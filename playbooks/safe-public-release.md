# Безопасный публичный релиз артефактов

Использовать, когда нужно открыть код, skills, prompts, templates, fixtures, datasets, workshop assets или другие материалы, возникшие в private corp repo, vendor/runtime environment либо смешанном рабочем каталоге.

Этот playbook не заменяет upstream PR workflow. Он управляет **outward publication** нового публичного artifact/package.

## Основной контракт

Публичный релиз разрешён только тогда, когда доказаны четыре вещи:

1. **Provenance:** известно, откуда взялся каждый публикуемый файл.
2. **Redistribution right:** лицензия/terms разрешают публикацию в выбранной форме.
3. **Security/privacy:** пакет не содержит secrets, runtime state, private paths, user data или vendor-protected assets.
4. **Public usability:** свежий публичный clone устанавливается/обнаруживается/запускается по документированной команде.

`ATTRIBUTION.md` не заменяет право на redistribution. Неизвестная лицензия означает blocked release.

## Когда применять

- экспорт skills из agent/runtime продукта;
- перенос reusable code из private repo в public repo;
- публикация templates, playbooks, prompts или starter kits;
- открытие course/workshop assets;
- публикация benchmark fixtures или datasets;
- выделение demo project из production/private codebase;
- сбор public plugin bundle из нескольких внутренних файлов;
- vendor/runtime export, в котором источник и права неоднозначны.

## Когда не применять

- upstream bugfix PR — использовать open-source PR workflow и regression-first gate;
- обычный public repo edit, если весь source уже принадлежит owner и не смешан с private/runtime state;
- tool evaluation или smoke test — это product/runtime owner;
- security vulnerability disclosure — использовать private security route;
- создание нового corp-* department — использовать `corp-new` после отдельного approval.

## Owner tree

Создавать минимум три слоя:

1. **Owner epic:** цель релиза, intended audience, scope, risk owner и acceptance.
2. **Inventory/review child:** source discovery, provenance, license, security classification, allowlist.
3. **Publish/verify child:** public repo/package creation только после approved allowlist; fresh-clone verification.

Tool evaluation, content launch и infrastructure mutation выносить к своим owner repos. Не смешивать их с redistribution decision.

## State machine

Allowed states:

- `DISCOVERED` — есть идея открыть artifact; inventory ещё не начат.
- `INVENTORY` — source/provider/companions собираются.
- `PROVENANCE-CLEARED` — source и version известны для allowlisted candidates.
- `LICENSE-CLEARED` — redistribution basis подтверждён.
- `SECURITY-CLEARED` — package classification и scans пройдены.
- `PACKAGE-READY` — staging tree собран только из allowlist.
- `APPROVED` — owner явно разрешил outward publication.
- `PUBLISHED` — публичный artifact создан.
- `VERIFIED` — fresh clone/install/discovery smoke прошёл.
- `MAINTAINED` — назначены owner, update policy и release cadence.

Block states:

- `BLOCKED-PROVENANCE`
- `BLOCKED-LICENSE`
- `BLOCKED-SECURITY`
- `BLOCKED-OWNER-APPROVAL`
- `NO-GO-VENDOR-PROTECTED`
- `NO-GO-MIXED-PRIVATE-DATA`

Каждый blocked status обязан иметь один `unblock_event`: источник найден, право получено, секрет удалён/rotated, owner approval, vendor permission либо explicit scope reduction.

## Release manifest

Создать `release-manifest.yaml` до копирования файлов в staging tree.

Минимальные поля для каждого artifact:

```yaml
release:
  name: example-public-package
  owner: serejaris
  intended_repository: serejaris/example-public-package
  status: INVENTORY
  source_issue: https://github.com/OWNER/PRIVATE-OR-OWNER-REPO/issues/N
  reviewer: null
  approved_at: null

artifacts:
  - id: example-skill
    source_provider: vendor-or-owner
    source_locator: redacted-stable-description
    source_version: commit-tag-or-product-version
    source_owner: owner-or-vendor
    license: MIT
    redistribution_basis: upstream-license
    modified: true
    modifications: sanitized paths and generalized configuration
    bundle_files:
      - skills/example/SKILL.md
      - skills/example/references/example.md
    excluded:
      - runtime logs
      - credentials
      - caches
    decision: REVIEW
    notes: null
```

Не записывать в public manifest private absolute paths, tokens, usernames, hostnames или internal URLs. Internal evidence может жить в owner issue.

## Inventory classification

Каждый найденный файл классифицировать ровно в одну группу:

| Класс | Решение |
|---|---|
| Owner-authored source | candidate после license/security review |
| Third-party redistributable source | candidate с preserved license/notices |
| Generated build artifact | обычно regenerate, не копировать |
| Runtime state / cache / queue | exclude |
| Logs / transcripts / histories | exclude либо отдельная explicit anonymization review |
| Credentials / tokens / cookies | exclude + rotate при exposure |
| User/customer/student data | exclude; aggregation требует отдельного privacy owner |
| Vendor-protected / unknown source | block |
| Mixed bundle | split; не публиковать целиком |
| Symlink / submodule / archive | inspect target/content before allowlist |

## Provenance gate

Для каждого candidate доказать:

- source provider/owner;
- source version, tag, commit или product version;
- original license/terms;
- companion files, без которых artifact неполон;
- modifications, сделанные owner;
- intended public license;
- основание redistribution.

Если файл «виден внутри runtime» — этого недостаточно. Visibility не доказывает ownership или redistribution right.

## License gate

Проверить:

- root license и per-file notices;
- dependency/vendor licenses;
- trademark/name restrictions;
- generated content terms;
- share-alike/copyleft obligations;
- запрет на redistribution, reverse engineering или extraction;
- совместимость intended public license с source licenses.

Unknown/missing/custom license → `BLOCKED-LICENSE` до правовой ясности или исключения artifact.

## Security and privacy gate

### Обязательная ручная классификация

Исключить:

- `.env*`, credentials, API keys, OAuth state, cookies;
- SSH keys, certificates, signing material;
- logs, queues, caches, sessions, histories;
- browser profiles и local storage;
- private/customer/student/user data;
- absolute private paths и internal hostnames;
- private GitHub/Granola/Notion/CRM links;
- internal issue IDs, если они раскрывают private structure;
- vendor binaries/assets без права redistribution;
- hidden files, archives and symlink targets без review.

### Staged-tree checks

Запускать на staging tree, а не только на source directory:

```bash
find . -type l -print
find . -type f -size +10M -print
rg -n --hidden -S '(api[_-]?key|secret|token|password|BEGIN [A-Z ]*PRIVATE KEY|Authorization: Bearer)' .
rg -n --hidden -S '(/Users/|/home/|C:\\Users\\|private-|corp-|localhost:[0-9]{2,5})' .
git diff --check
```

Если доступны approved scanners:

```bash
gitleaks detect --no-git --source .
trufflehog filesystem --no-update .
```

Scanner success не заменяет ручной review. Любой найденный действующий secret → stop, remove, rotate, document internally, rerun scans.

## Allowlist-first packaging

Не копировать source directory целиком с последующим удалением «лишнего».

1. Manifest формирует explicit file allowlist.
2. Каждый bundle переносится полностью: main file + references + examples + scripts + required notices.
3. Generated files регенерируются из allowlisted source, когда возможно.
4. Staging tree создаётся в новом clean directory/branch.
5. Проверяется `git status --short` и полный staged file list.
6. Owner сравнивает staged tree с manifest.

Пример:

```bash
mkdir -p /tmp/release-stage
rsync -a --files-from=allowlist.txt SOURCE_ROOT/ /tmp/release-stage/
cd /tmp/release-stage
find . -type f -print | sort
```

## Documentation gate

Public package должен содержать:

- понятный `README.md`;
- install/use command;
- source attribution и preserved notices;
- public license;
- security contact;
- compatibility/version notes;
- known limitations;
- maintenance owner;
- changelog/release note, если artifact versioned.

Не обещать compatibility, которую smoke test не доказал.

## Approval gate

Перед outward write показать owner краткий dry run:

```text
Release: <name>
Target: <public owner/repo or registry>
Artifacts allowed: <count/list>
Artifacts blocked/excluded: <count/reasons>
Source licenses: <summary>
Security scans: <commands/results>
Fresh-clone verification plan: <command>
Open risks: <none or list>
Proposed action: create repo | publish package | update existing release
```

Без явного approval статус остаётся `PACKAGE-READY` или `BLOCKED-OWNER-APPROVAL`.

## Publish gate

После approval:

1. Создать/обновить public repo из clean staging tree.
2. Commit message связывает owner issue: `refs OWNER/corp-opensource#N`.
3. Проверить repository visibility, default branch и license.
4. Не переносить private issue body/comments в public repo.
5. Не force-push и не переписывать unrelated public history.

## Fresh public boundary verification

Проверять как внешний пользователь:

```bash
workdir=$(mktemp -d)
git clone --depth 1 https://github.com/OWNER/REPO.git "$workdir/repo"
cd "$workdir/repo"
```

Далее выполнить документированную команду:

- plugin/skill discovery;
- package install/import;
- CLI `--help` / smoke;
- example/test fixture;
- link/read path without authentication.

Повторить private-path/secret scan на fresh clone. Status `VERIFIED` возможен только после этого.

## Release evidence card

В owner issue записать:

```text
release_status: VERIFIED
public_url: https://github.com/OWNER/REPO
source_version: ...
manifest: release-manifest.yaml
license_basis: ...
security_checks: ...
publication_commit: ...
fresh_clone_command: ...
fresh_clone_result: PASS
maintenance_owner: ...
next_review: YYYY-MM-DD
```

## Failure patterns → rules

| Failure | Rule |
|---|---|
| Скопировали runtime folder целиком | allowlist-first; clean staging tree |
| Attribution есть, license неизвестна | attribution is not permission; block |
| Опубликован только `SKILL.md`, references потеряны | release whole bundle |
| Secret scanner green, private path остался | manual classification + path scan |
| Работает в owner checkout, fresh clone сломан | public-boundary smoke обязателен |
| Evaluation и release смешались | route by risk owner; separate issues |
| Public repo создан до inventory | no outward mutation before PACKAGE-READY + approval |
| Vendor asset виден пользователю | visibility does not imply redistribution right |
| Release заброшен после публикации | maintenance owner + next review required |

## Связи

- Reusable skill: `serejaris/personal-corp-skills/skills/safe-public-release`.
- Source project: issues #88–90.
- Operating model: issue #93.
- Implementation tracker: issue #92.
