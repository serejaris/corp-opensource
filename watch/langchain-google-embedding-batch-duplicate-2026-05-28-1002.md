# LangChain Google embedding batch duplicate watch, 2026-05-28 10:02 UTC

Tracker: [#52](https://github.com/serejaris/corp-opensource/issues/52)

Tracker comment: [#52 10:02 LangChain duplicate correction](https://github.com/serejaris/corp-opensource/issues/52#issuecomment-4562901015)

Required `open-source-bug-scouting` / `open-source-pr-workflow` skills are not installed in this environment, so this cycle used the documented fallback: parent live GitHub gates, 6 read-only subagent roles, and parent duplicate/PR correction.

No upstream PR/comment/ping was made.

## Result

`next_status: WATCH`

Upstream action count: `0`.

Runner action count: `0`.

## Live gates

| Lane | Live result | Decision |
|---|---|---|
| `langchain-ai/langchain#37728` | Open `bug`/`external`, created `2026-05-28T09:57:10Z`, unassigned, 1 contributor self-claim comment at `2026-05-28T10:00:06Z`. Reporter says `langchain_google_genai.GoogleGenerativeAIEmbeddings.embed_documents()` returns 1 vector for N texts with `gemini-embedding-2`. | Demoted from fresh `LEAD` to `WATCH / duplicate-covered`. |
| `langchain-ai/langchain-google#1704` | Open issue in the actual package repo for the same `GoogleGenerativeAIEmbeddings` batch input behavior. | Exact/strong prior issue cover. |
| `langchain-ai/langchain-google#1708` | Open PR: `fix(genai): wrap batch texts in Content objects for correct embedding count`, updated `2026-05-22T04:18:07Z`. | Exact/strong PR cover; no racing PR/comment. |
| `anthropics/claude-code#63070` | Open enhancement/i18n slash-command localization; adjacent localization issues exist. | `WATCH`, not bug-candidate. |
| `openai/codex#24888` | Open UX proposal for model picker shortcut. | `WATCH`. |
| `anthropics/claude-code#63069` | Open but labeled duplicate for VS Code/1M context quota behavior. | `NO-GO / duplicate`. |
| `openai/codex#24887` | Quota/account behavior; reporter says logout/login made quota look normal. | `NO-GO / service-account`. |

## 6-role synthesis

- Fresh discovery found `langchain#37728` as a plausible AI-native lead, but parent live gates located the implementation in `langchain-ai/langchain-google`, not the main `langchain` monorepo.
- Duplicate/PR gate found existing issue `langchain-google#1704` and open PR `langchain-google#1708`, so this is not a new contribution lane.
- Active candidates (`claude-peers-mcp#64`, `vercel/ai#15652`, `probe#568`, `gemini-cli#27503`, `CLIProxyAPI#3594`) did not materially change and remain blocked by runner/process gates.
- Claude/Codex/opencode fresh high-churn items remain product, feature, duplicate, assigned, or meta-watch lanes.

## Critique

- Factology/duplicates: `langchain#37728` is real and fresh, but covered by the package-specific repo issue/PR; do not treat the main-repo issue as uncovered.
- Process gates: no upstream action because duplicate cover and contributor self-claim are both live; runner remains unavailable via [#10](https://github.com/serejaris/corp-opensource/issues/10).
- Actionability: record internal correction only. Re-enter only if `langchain-google#1708` closes unmerged or merges incompletely and current-main repro proves a remaining gap.
