# AionUi / AionCore #3074 STT multipart contract, 2026-05-28

Date: 2026-05-28 05:00 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4560981873)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this cycle used the documented fallback: parent live GitHub gates, 6 read-only subagents, then internal process critique. No upstream PR/comment was made.

## Scope

Fresh issue discovery across high-value AI-native / Paperclip-like repos after the pydantic runtime-timeout refresh. The useful candidates were:

- [`iOfficeAI/AionUi#3074`](https://github.com/iOfficeAI/AionUi/issues/3074): STT `/api/stt` returns `400 Bad Request` before contacting an OpenAI-compatible transcription provider.
- [`iOfficeAI/AionUi#3075`](https://github.com/iOfficeAI/AionUi/issues/3075): STT GUI config is not persisted.
- [`OpenHands/OpenHands#14563`](https://github.com/OpenHands/OpenHands/issues/14563): host-global skills are invisible to `openhands serve` containers.
- [`cline/cline#9906`](https://github.com/cline/cline/issues/9906): checkpoint restore keeps image attachments stuck in input.

Final `next_status`: `CANDIDATE-needs-runner-repro` for `AionUi#3074`.

## Parent Live Gates

- `iOfficeAI/AionUi`: Apache-2.0, default branch `main`, about 26.9k stars, latest release `v2.1.5` on 2026-05-27, `CONTRIBUTING.md` present.
- `iOfficeAI/AionCore`: public sibling backend, latest release `v0.1.14` on 2026-05-27; no `CONTRIBUTING.md` found by contents API.
- `AionUi#3074`: open `bug`, unassigned, no comments. Reporter says WebUI/Desktop STT recording reaches bundled backend, which returns `/api/stt` `400` with `latency_ms=0`; provider reverse proxy sees only `GET /v1/models`, never `POST /audio/transcriptions`; direct provider curl succeeds.
- `AionUi#3075`: open `bug`, unassigned, no comments. Adjacent STT config persistence issue; do not merge into the same root cause without repro.
- Duplicate PR gate: exact open searches for `3074 STT 400 Bad Request api stt`, `3075 speechToText persisted settings mic icon`, AionCore `STT fileName audio multipart speechToText`, and `api/stt missing file field` returned empty. AionUi all-state STT PRs are older merged feature/fix PRs, not exact cover.
- Runner gate: no repro/test was run. `corp-opensource-runner` is still unavailable; local environment is not the stable dedicated runner required for AionUi/AionCore STT repro.

## Source Gates

Source evidence is strong enough for internal candidate status but not PR-ready:

- `AionUi` `packages/desktop/src/renderer/services/SpeechToTextService.ts` web path posts `FormData` fields `audio`, `mimeType`, and optional `languageHint` to `/api/stt`.
- `AionCore` `crates/aionui-shell/src/routes.rs` extracts `/api/stt` multipart fields named `file`, `fileName`, `mimeType`, and optional `languageHint`; missing `file`, `fileName`, or `mimeType` returns `400`.
- `AionCore` `crates/aionui-shell/src/stt_openai.rs` forwards accepted audio upstream as OpenAI-compatible multipart `file` plus `model`, so reporter's direct curl shape matches the provider side.
- `AionUi#3074` symptom therefore matches a likely web multipart contract mismatch: renderer sends `audio`, backend requires `file` and `fileName`.

Risk: desktop IPC path is separate and sends `audioBuffer`, `file_name`, `languageHint`, `mimeType`; it should not be assumed to share the exact web multipart bug. A `400` could also come from content-type, auth/session, or a different route path, so runner fail-before is mandatory.

## Candidate Map

| Lane | Live result | Decision |
|---|---|---|
| `AionUi#3074` | Fresh open STT bug with precise provider-bypass evidence; source gate shows likely multipart field mismatch; no exact PR. | `CANDIDATE-needs-runner-repro`; best next Aion runner lane. |
| `AionUi#3075` | Fresh open adjacent STT settings persistence issue; source shows `configService.set('tools.speechToText', next)` but no repro or shared root cause. | `WATCH / adjacent`; do not bundle with `#3074` yet. |
| `OpenHands#14563` | Open host-global skills mount issue; we already asked maintainers whether a narrow `openhands serve` mount option is welcome. | `COMMENT-FIRST / wait-maintainer-direction`; no runner slot yet. |
| `cline#9906` | Confirmed checkpoint attachment issue, but old thread includes contributor claim "Created a PR"; exact search did not find it now. | `WATCH`; ownership/race-sensitive and older than Aion lane. |

## 6-Role Synthesis

- Factology: `#3074` aligns with source: web renderer field name `audio` does not match backend `file` requirement, and backend also requires `fileName`.
- Duplicate/race: no exact open PR was found in AionUi or AionCore. Old merged STT PRs are context, not cover.
- Process: upstream action is blocked. No runner-backed fail-before, no stable AionUi/AionCore repro, and no PR-ready patch gate.
- Actionability: minimal runner repro is to post the current web payload to `/api/stt` and confirm `400` before provider call, then post `file` + `fileName` + `mimeType` and confirm backend validation passes.
- Scope: Aion voice/STT is a good Paperclip-like fit because it touches the human-agent loop in a local multi-agent UI; OpenHands skills and Cline checkpoints stay watch lanes for now.
- Tracker/delta: material new candidate; create this watch note, update `#52`, and add a README summary. Dedicated issue is not needed before runner evidence.

## Decision

`next_status: CANDIDATE-needs-runner-repro`

Upstream action count: `0`.

Runner action count: `0`.

Re-entry triggers:

1. `corp-opensource-runner` becomes available, then run the `/api/stt` multipart fail-before and corrected-shape control.
2. `AionUi#3074` receives maintainer source guidance, assignment, or linked PR.
3. AionUi/AionCore gets a new STT PR; repeat exact duplicate gate before any action.
