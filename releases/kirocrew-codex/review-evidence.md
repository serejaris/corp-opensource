# KiroCrew-Codex · evidence card

```text
release_status: BLOCKED-LICENSE
publication_fact: PUBLISHED before canonical review
public_url: https://github.com/serejaris/KiroCrew-Codex
public_release: https://github.com/serejaris/KiroCrew-Codex/releases/tag/v0.2.0-codex.1
source_version: 8e988362db845acc473f56e7f340ccb2b8c47d51
publication_commit: 59ab5fcc11d37c07b1924d5d7e32b49b6e7e8550
manifest: releases/kirocrew-codex/release-manifest.yaml
allowlist: releases/kirocrew-codex/allowlist.txt
license_basis: Apache-2.0, currently blocked on section 4(b) notices and trademark-excluded branding
security_checks: tracked-tree classification, private-path/manual secret scans, GitHub secret scanning, Dependabot, repository CI, forced-env privacy probes
fresh_clone_result: BLOCKED after successful build, CLI, doctor, privacy and live-model smoke; public docs/license/security gates remain
maintenance_owner: serejaris
next_review: 2026-08-14
```

## Six-role review

| Роль | Gate | Ключевой вывод |
|---|---|---|
| Provenance и license | `BLOCKED-LICENSE` | Provenance доказан; modification notices и product marks блокируют replacement release. |
| Security и privacy | `BLOCKED-SECURITY` | Egress закрыт; UI/docs обещания, local usage retention/mode и public workflow hygiene требуют исправления. |
| Public boundary | `BLOCKED-PUBLIC-DOCS` | Fresh build проходит; install/Windows docs и release workflow создают неверный публичный путь. |
| Runtime verification | `VERIFIED` | Fresh clone, installed app source match, doctor, privacy tests и bounded live Codex smoke прошли. |
| Process | `BLOCKED-LICENSE` | Owner tree корректен; backfill сохраняет отсутствие pre-publication dry run и требует новый corrective tag. |
| Maintenance и claims | `BLOCKED — NO PUBLIC ANNOUNCEMENT` | Нужны rebrand, telemetry-safe default, отдельный release workflow, signed tag и честные distribution claims. |

## Проверенные факты

- Тег указывает на exact commit, release не содержит binary assets.
- У public Build run есть три скачиваемых unsigned CI artifact; source-only claim требует исправления.
- Public fork relation с upstream подтверждён.
- Exact tree содержит 4 416 tracked paths, без symlink и submodule.
- Fresh clone собрал frontend/backend и прошёл CLI/doctor.
- `KIROCREW_TELEMETRY=1` и заданный OTLP endpoint не включили outbound telemetry в fork-specific probes.
- Default embedding-model URL остаётся privacy blocker: проектная документация использует CDN request как first-install signal.
- Живой Codex smoke использовал существующий ChatGPT login и вернул ожидаемую строку.
- GitHub secret scanning показывал только два synthetic AWS fixtures; оба разрешены как `used_in_tests` после ручной проверки.
- Локальные сканеры `gitleaks`, `trufflehog`, `detect-secrets`, `exiftool` отсутствовали; зелёный результат для них не заявляется.

## Публичные claims до replacement release

Разрешено утверждать только: source-only experimental fork существует; exact tag собирается из fresh clone; Codex smoke работает через существующий ChatGPT login; beacon, install receipt, OTLP exporter и upstream updater выключены в проверенном SHA. Полное отсутствие продуктовых сигналов, готовность к production, license clearance, удобный Windows install и официальный статус не заявляются.
