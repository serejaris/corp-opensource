# KiroCrew-Codex · release gate

**Статус:** `BLOCKED-LICENSE`
**Owner issue:** [#94](https://github.com/serejaris/corp-opensource/issues/94)
**Exact public tag:** `v0.2.0-codex.1` → `59ab5fcc11d37c07b1924d5d7e32b49b6e7e8550`

Публичный source-only prerelease появился до запуска `safe-public-release`. Этот пакет фиксирует governance backfill без ретроактивного заявления о пройденном pre-publication approval.

## Пройдено

- [x] Источник, upstream base, fork relation, tag и publication SHA зафиксированы.
- [x] Exact tracked tree записан в `allowlist.txt`: 4 416 путей.
- [x] В tree нет symlink, submodule, installer/archive и файлов больше 10 MiB.
- [x] Fresh public clone собирается командой `make build`.
- [x] CLI help и doctor проходят; Codex использует существующий ChatGPT login.
- [x] Живой запрос через `gpt-5.6-sol` вернул `CORP_OPENSOURCE_LIVE_OK`.
- [x] Forced-env privacy probes прошли `3/3`.
- [x] Community-fork update tests прошли `2/2`.
- [x] GitHub CI прошёл `24/24`, Build прошёл `3/3`.
- [x] Два secret-scanning alert подтверждены как synthetic test fixtures и закрыты с resolution `used_in_tests`; открытых alert — 0.
- [x] Dependabot open alerts — 0.

## Блокеры

- [ ] Дать каждому изменённому upstream-файлу заметное указание на модификацию по Apache-2.0 §4(b).
- [ ] Убрать из product/repository/app identity лицензированно неразрешённые Kiro/Kiro Crew marks и рискованное использование Codex в названии приложения.
- [ ] Переписать UI и docs так, чтобы они честно сообщали об отключённом outbound telemetry.
- [ ] Убрать attributable first-install signal через default KiroCrew CDN: локальный GGUF, явный opt-in либо нейтральный источник с честным network disclosure.
- [ ] Для локального usage log добавить явное управление, retention/pruning и режим файла `0600`.
- [ ] Убрать upstream AWS signing/CDN identifiers из public workflow boundary.
- [ ] Прекратить публичную выдачу unsigned desktop/wheel CI artifacts либо явно отделить их от release distribution и проверить их лицензии.
- [ ] Закрепить immutable SHA для scheduled GitHub Actions.
- [ ] Исправить Windows/install docs и воспроизводимый checkout exact tag.
- [ ] Убрать `temp-screenshots/` из replacement release tree.
- [ ] Решить историю с корпоративным email в upstream ancestry через approved history/provenance strategy.
- [ ] Подписать replacement tag и включить tag protection/release gate.
- [ ] Повторить scanners; отсутствие `gitleaks`, `trufflehog`, `detect-secrets`, `exiftool` зафиксировано как ограничение текущего окружения.

## Следующий gate

Единственное unblock event: debranded и license-compliant replacement tag проходит полный `safe-public-release` с fresh-clone verification. Точный публичный target и имя требуют отдельного подтверждения владельца.

Пост в чат вайбкодеров отправляется после зелёного replacement release и отдельного подтверждения exact preview.
