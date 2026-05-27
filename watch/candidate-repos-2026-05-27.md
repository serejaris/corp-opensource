# Candidate Repos Snapshot - 2026-05-27

Source: live `gh search repos` on 2026-05-27.

## Strong First Lanes

| Repo | Stars | Why watch |
|---|---:|---|
| [NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) | 168890 | Already reproduced and contributed; agent runtime bugs are close to our own usage. |
| [anomalyco/opencode](https://github.com/anomalyco/opencode) | 165776 | Open-source coding agent; likely overlap with Hermes/Codex workflows. |
| [google-gemini/gemini-cli](https://github.com/google-gemini/gemini-cli) | 104626 | Agent CLI with active user surface and likely regression issues. |
| [langchain-ai/langchain](https://github.com/langchain-ai/langchain) | 137718 | Large AI runtime, many integration bugs, strong test culture. |
| [microsoft/markitdown](https://github.com/microsoft/markitdown) | 125341 | Conversion bugs are fixture-friendly and PR-scoped. |
| [firecrawl/firecrawl](https://github.com/firecrawl/firecrawl) | 124825 | AI scraping/runtime bugs are reproducible and visible. |
| [open-webui/open-webui](https://github.com/open-webui/open-webui) | 138791 | Active AI UI, many user-reported bugs. |
| [langflow-ai/langflow](https://github.com/langflow-ai/langflow) | 148791 | Agent workflow builder; likely runtime integration bugs. |

## Six-subagent scouting synthesis - 2026-05-27

| Rank | Candidate | Upstream signal | Why |
|---:|---|---|---|
| 1 | [NousResearch/hermes-agent#32963 follow-up](https://github.com/serejaris/corp-opensource/issues/6) | Broad `TypeError` recovery may mask app-side callback errors | Done: repro confirmed, upstream PR [#32999](https://github.com/NousResearch/hermes-agent/pull/32999) opened. |
| 2 | [anomalyco/opencode#29428](https://github.com/anomalyco/opencode/issues/29428) | Bedrock/LiteLLM Task subagents fail with `UnsupportedParamsError`; `tools=` not passed | Real remaining gap, but no fresh PR yet: similar PR #29474 was closed unmerged on 2026-05-27. Ask maintainer what was wrong before reopening. |
| 3 | [firecrawl#3614](https://github.com/firecrawl/firecrawl/issues/3614) | Cancel crawl floods webhook with `crawl.failed` for every queued URL | Good API semantics bug with likely unit/integration test surface. |
| 4 | [google-gemini/gemini-cli#27466](https://github.com/google-gemini/gemini-cli/issues/27466) | `-p/--print` returns no stdout on Windows despite API answer | High reputation target, but requires Windows repro or diagnostic-first PR. |
| 5 | [microsoft/markitdown#1894](https://github.com/microsoft/markitdown/issues/1894) | `IpynbConverter.accepts()` crashes on non-ASCII bytes with `UnicodeDecodeError` | Repro confirmed, but no-go for new PR now: #1895 open and #1910 closed unmerged. Left duplicate-triage comment on #1895. |

Repo-fit conclusion:

- Best balance: `firecrawl/firecrawl`.
- Best reputation: `anomalyco/opencode` and `google-gemini/gemini-cli`.
- Fastest clean first merge lane was `microsoft/markitdown`, but #1894 is occupied by duplicate PRs; keep as upstream-watch until maintainer asks for a split PR.
- Existing relationship lane: `NousResearch/hermes-agent`.

Decision: Hermes hardening moved to upstream PR. MarkItDown #1894 moved to upstream-watch/no-go. Opencode #29428 moved to maintainer-signal watch because #29474 already attempted the likely fix and was closed unmerged. Next clean implementation candidate: Firecrawl #3614 unless opencode maintainer clarifies an acceptable narrower patch.

## Lower Priority Despite Stars

| Repo | Reason |
|---|---|
| `public-apis/public-apis`, `awesome-python`, `free-programming-books` | Mostly curation/docs, weak runtime bug surface. |
| `microsoft/vscode`, `microsoft/TypeScript`, `angular/angular` | High impact but high process overhead; use only for very well-scoped bugs. |
| `freeCodeCamp/freeCodeCamp`, roadmaps/interview repos | Product/curriculum surface, slower bugfix loop for our goal. |
