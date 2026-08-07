# VibecodersCrew · release gate

**Статус:** `PUBLISHED-VERIFYING`
**Owner issue:** [#94](https://github.com/serejaris/corp-opensource/issues/94)
**Exact public tag:** `v1.0.0` → `4960452573098586700382fcc98dc4eecdb1a106`
**Supersedes:** `KiroCrew-Codex` / `v0.2.0-codex.1`

## Пройдено в corrective release

- [x] Product/repo/app identity → VibecodersCrew / serejaris/vibecoderscrew / tech.serejaris.vibecoderscrew
- [x] Apache-2.0 §4(b) notices on modified source + MODIFICATIONS.md + NOTICE
- [x] Default embedding CDN first-install signal removed
- [x] Product telemetry remains hard-disabled
- [x] Public Build workflow no longer uploads unsigned wheel/desktop artifacts
- [x] `temp-screenshots/` removed from tracked tree
- [x] Source-only GitHub Release `v1.0.0` published
- [x] Repo renamed from KiroCrew-Codex → vibecoderscrew

## Pending verification

- [ ] Fresh clone of exact tag builds
- [ ] CLI/doctor/live Codex smoke
- [ ] Forced-env privacy probes
- [ ] Confirm CI green without public binary artifacts
- [ ] Secret/Dependabot open alerts
- [ ] Exact Telegram preview owner approval

## Known residual technical identifiers

Documented, not product identity:
- Python package path `kiro_crew`
- CLI alias `kirocrew`
- Default data home `~/.kiro/crew` (override `KIROCREW_HOME`)
