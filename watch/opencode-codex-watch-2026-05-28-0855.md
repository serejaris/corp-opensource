# Opencode/Codex watch, 2026-05-28 08:55 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this used the documented fallback: parent live GitHub gates, reused 6 read-only subagent roles, and focused duplicate/PR checks before status selection.

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

Runner gate: dedicated `corp-opensource-runner` is still unavailable via [#10](https://github.com/serejaris/corp-opensource/issues/10). CT216 remains Hermes-only and was not used.

## Material deltas

- `anomalyco/opencode#29704`: closed at `2026-05-28T08:51:45Z` after the reporter agreed that fixing `#21939` will resolve the GPU impact. This moves the lane from weak `WATCH` to `NO-GO / duplicate-covered`.
- `anomalyco/opencode#28150`: fresh comment links open PR `#28689` as the fix for granular read permission deny rules. PR `#28689` is open, non-draft, `MERGEABLE`; local status is `WATCH / covered-by-PR`.
- `anomalyco/opencode#16962`: fresh confirmation says SSH clipboard copy fails from Warp to a Mac mini while iTerm2 succeeds. This narrows terminal/OSC52 behavior but stays `WATCH` because the issue is assigned and duplicate-linked to `#15907/#6586/#8237`.
- `openai/codex#24884`: external contributor edited their intent comment after searching public code and not finding the Windows App host-side WSL bootstrap path. The comment now asks maintainers whether the code is public and offers a docs/diagnostic PR fallback. This reinforces `WATCH / product-private-surface`, not a candidate.

## Parent live gates

`opencode#29704`:

- State: closed.
- Assignee before close: `Hona`.
- Duplicate bot pointed to `#21939` for disabling/reducing animations.
- Reporter comment: fixing `#21939` will resolve the GPU impact too.
- Decision: `NO-GO / duplicate-covered`.

`opencode#28150`:

- State: open, assignee `kitlangton`.
- Fresh comment at `2026-05-28T08:51:05Z` says PR `#28689` fixes the granular read permission deny bug by changing `*` to single-segment matching and adding `**` globstar semantics.
- PR `#28689`: open, non-draft, `MERGEABLE`, guideline bot satisfied.
- Decision: `WATCH / covered-by-PR`.

`opencode#16962`:

- State: open, assignee `kommander`.
- Duplicate bot already linked `#15907/#6586/#8237`.
- Fresh comment at `2026-05-28T08:50:04Z` confirms Warp-to-Mac-mini failure while iTerm2 succeeds.
- Decision: `WATCH`; no cold PR/comment.

`codex#24884`:

- State: open, label `app`, no assignee.
- Fresh edited external comment says public repo search did not reveal the Windows App WSL bootstrap path and asks maintainers for a pointer or confirmation that code is outside the public repo.
- Decision: `WATCH`; no `COMMENT-FIRST` because another contributor is already asking maintainers and no public source path or runner evidence exists.

## 6-role synthesis

- Fresh discovery found no new post-08:48 source-level candidate in the popular target set. Broader GitHub search returned mostly low-fit or malformed/scheduled noise.
- Active refresh found `opencode#29704` closed as duplicate-covered and no material transitions for `claude-peers-mcp#64`, `CLIProxyAPI#3594`, `mem0#5284`, `pydantic-ai#5670`, `vercel/ai#15665/#15652`, `probe#568`, or `gemini-cli#27517`.
- Duplicate/race kept `claude-peers-mcp#64` as `CANDIDATE`, `CLIProxyAPI#3594` as uncovered but protocol-heavy `LEAD`, and `codex#24884` as `WATCH / NO-GO for us now`.
- Process gates rejected `COMMENT-FIRST` and `PR-READY` because mandatory skills are unavailable, runner `#10` is unavailable, CT216 is Hermes-only, and current fresh deltas are duplicate-covered, assigned, or product-private-surface.
- Actionability keeps `claude-peers-mcp#64` as the best compact source-level `CANDIDATE-needs-runner-validation`; runner backlog order remains `vercel/ai#15652`, `probelabs/probe#568`, `gemini-cli#27503`.
- Synthesis selected `next_status: WATCH` because this cycle only closes/demotes watch lanes and does not promote a new candidate.

## 3-role critique

- Factology/duplicates: approved after parent verification of `opencode#29704` closure, `opencode#28150/#28689`, `opencode#16962`, and `codex#24884`.
- Process gates: approved internal watch/tracker update only. Upstream PR/comment count stays `0`.
- Actionability: no runner work. Next promotion still requires runner-backed validation of `claude-peers-mcp#64` or an existing runner backlog candidate once `corp-opensource-runner` is provisioned.
