# Repo Bug Scouting Playbook

Use this to find the next open-source contribution target.

## Inputs

- Language/runtime lanes: Python, TypeScript, AI agents, data conversion, self-hosted tools.
- Time window: last 7 days for repo activity, last 30 days for issues.
- Minimum signal: active maintainers plus reproducible bug.

## Search Commands

```bash
gh search repos --language Python --stars '>50000' --sort stars --limit 20 --json fullName,description,stargazersCount,updatedAt,url
gh search repos --language TypeScript --stars '>50000' --sort stars --limit 20 --json fullName,description,stargazersCount,updatedAt,url

gh search issues 'repo:OWNER/REPO is:issue is:open label:bug updated:>=YYYY-MM-DD' --limit 30
gh search issues 'repo:OWNER/REPO is:issue is:open regression OR crash OR TypeError OR broken' --limit 30
gh search prs 'repo:OWNER/REPO is:pr is:open bug OR regression OR crash' --limit 30
```

## Subagents

Run three parallel checks before picking a bug:

1. Repo fit: stars, recent activity, contribution rules, test harness, maintainer response speed.
2. Bug signal: duplicates, reactions, user impact, reproduction details.
3. Patchability: likely files, existing tests, local setup cost, risk of broad refactor.

## Output

Create one issue per candidate with:

- upstream repo and issue/PR links;
- score and why;
- reproduction state;
- likely patch surface;
- exact next command or investigation step.

