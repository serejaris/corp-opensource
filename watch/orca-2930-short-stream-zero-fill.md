# Orca #2930 short stream zero-fill, 2026-05-28

Tracker: [#82](https://github.com/serejaris/corp-opensource/issues/82)
Upstream: [stablyai/orca#2930](https://github.com/stablyai/orca/issues/2930)
Cycle: [AI-native popular repo universe, cycle 31](ai-native-popular-repos-paperclip-scouting-2026-05-28-cycle-31-repo-universe.md)

## Scope

Issue-level promotion from the 2026-05-28 coding-agent control-plane / MCP infra repo-universe cycle.

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this cycle used the documented fallback: 6 read-only scouting roles, parent live GitHub gates, then 3-role critique.

No upstream PR/comment/ping was made.

## Parent live gates

Live state at `2026-05-28 00:10-00:25 UTC`:

- Repo `stablyai/orca` is active, default branch `main`, MIT, latest release `v1.4.30`, pushed `2026-05-28`.
- Issue `#2930` is open, unassigned and unlabeled.
- Reported class: short or missing remote file stream chunk can resolve as zero-filled bytes instead of rejecting, creating silent data corruption.
- Exact PR search by `#2930`, `zero-fill`, `stream chunks`, `ssh-filesystem-stream-reader`, `readFileViaStream`, and `StreamProtocolError` found no covering PR. The visible `#852` hit is unrelated e2e test work.
- Neighbor duplicate pressure is high: `#2928 -> #2940`, `#2929 -> #2941`, and many same-hour Orca bug lanes are covered within minutes.
- Process gate: `.github/CONTRIBUTING.md` exists; PR template asks for AI review, security audit, cross-platform notes and test evidence. Checks: `pnpm lint`, `pnpm typecheck`, `pnpm test`, `pnpm build`. Package requires Node 24 / pnpm 10.

## Source gate

Parent source read on upstream `main`:

- `src/main/ssh/ssh-filesystem-stream-reader.ts` allocates `Buffer.alloc(totalSize)`.
- `handleChunk` decodes `params.data` and copies bytes into the preallocated buffer at `seq * STREAM_CHUNK_SIZE`.
- It rejects out-of-order chunks and chunk overflow, but does not validate decoded length for non-final or final chunks.
- `handleEnd` checks only `receivedChunks === totalChunks`, then returns the whole preallocated buffer.

Hypothesis: a short final chunk with the expected sequence/count leaves zero-filled bytes in the buffer and still resolves. This matches the reporter's repro and is not yet runner-proven.

## 6-role synthesis

- Repo-fit: Orca is a strong Paperclip-like coding-agent IDE/worktree target, but high duplicate velocity means no blind PR.
- Issue-quality: `orca#2930` is the strongest lane; `#2931` is secondary; `emdash#2110` is only a lead.
- Duplicate/PR race: no exact `#2930` PR found, but adjacent Orca bug lanes are already covered, so repeat duplicate search is mandatory before any upstream action.
- Process/license: Orca is process-safe, MIT, with Node 24 / pnpm 10 checks and strict PR-template review/security notes.
- Runner/repro: `agent-deck#1212` is easier in runner, but lower impact; `orca#2930` needs a focused Node test and has higher data-loss risk.
- Synthesis: promote `stablyai/orca#2930` to internal `CANDIDATE`; update repo-card/watch; upstream action count `0`.

## 3-role critique

- Factology/duplicates: supports internal `CANDIDATE`; source path confirms the plausible zero-fill behavior; no exact covering PR was found.
- Process gates: internal tracker is allowed, but no upstream comment/PR before runner proof and PR-template compliance notes.
- Actionability: `CANDIDATE`, not `COMMENT-FIRST` because upstream issue already contains impact, location, repro and suggested fix; not `PR-READY` because no fail-before or patch has run.

## Decision

`next_status: CANDIDATE`

Upstream action count: `0`.

Reason: live open issue, high-impact data-corruption class, exact duplicate PR not found, and parent source check confirms a narrow plausible bug path. The lane remains blocked on dedicated runner fail-before and final duplicate gate.

## Regression card

Target: `src/main/ssh/ssh-filesystem-stream-reader.ts` and a focused SSH stream reader test.

Fail-before shape:

1. Stub `mux.request('fs.readFileStream')` with `totalSize = STREAM_CHUNK_SIZE * 2`, `resultEncoding = 'base64'`, `isBinary = true`.
2. Emit `fs.streamChunk` seq `0` with `STREAM_CHUNK_SIZE` bytes.
3. Emit `fs.streamChunk` seq `1` with one byte.
4. Emit `fs.streamEnd`.
5. Expected contract: reject with `StreamProtocolError`; suspected current behavior: resolve with zero-filled tail.

Also test a short non-final chunk. Fix expectation: track decoded byte count / expected coverage and reject `streamEnd` unless `bytesReceived === totalSize`.

## Runner note

Runner was not used in this cycle because this was scouting + source-gate promotion, not a PR-ready implementation pass. Next step requires dedicated `corp-opensource-runner` with Node 24 / pnpm 10. CT `216` remains Hermes-only and was not used.

## Promotion triggers

- `PR-READY`: runner-backed failing regression on current `main`, minimal patch, green focused tests, fresh duplicate search, and PR-template review/security/cross-platform notes.
- `COMMENT-FIRST`: maintainer asks for direction or runner evidence contradicts/extends the current issue.
- `NO-GO`: exact upstream PR appears, issue closes as fixed, or source/repro disproves the zero-fill behavior.
