# Черновик docs-раздела для gastownhall/beads: bd при удалённом Dolt-сервере

Статус: черновик, наружу не отправлялся. Описание и вердикт — [beads-docs-remote-dolt-2026-10-12.md](beads-docs-remote-dolt-2026-10-12.md). Бид `cos-e4x.2`.

Куда класть в дереве апстрима: `docs/architecture/dolt.md`, новый подраздел `### Remote server: latency and running bd next to the database` сразу после подраздела «Server Mode (Multi-Writer / Orchestrator)» и перед `## Maintenance`. Запасной вариант, если мейнтейнеры не хотят растить dolt.md: отдельная страница `docs/architecture/remote-server.md` плюс строка в `docs/docs.json` (группа Architecture). Ссылка-указатель из `docs/reference/troubleshooting.md` § Performance Issues (подраздел «Slow commands against a remote server», 3 строки со ссылкой на якорь).

Всё ниже границы — текст для PR (английский).

---

### Remote server: latency and running bd next to the database

Server mode works against a Dolt server on another machine (`BEADS_DOLT_SERVER_HOST`, or `dolt.host` in `.beads/config.yaml`). Every bd command is a short-lived process that opens its own connections and then runs a series of SQL statements one after another. Each statement waits for the previous answer, so the network round-trip time (RTT) is paid once per statement, not once per command.

#### What it costs

Measured with bd 1.3.0 against a Dolt server on another continent (RTT 0.24 s through an SSH tunnel):

| Where bd runs | `bd show <id>` |
|---|---|
| On the laptop, Dolt server remote | 9.9 s |
| On the Dolt host, started over a reused SSH connection | 0.85 s |
| On the Dolt host, in a local shell | 0.17 s |

The cost scales with RTT, not with the size of the result. A packet-counted accounting in [#6114](https://github.com/gastownhall/beads/issues/6114) found 44 to 48 awaited round-trips for even the cheapest commands on 1.2.1 (`bd close`: 150). At 0.24 s that is 10 s or more before the command does any work of its own. Other reports with 0.1 to 0.25 s RTT: [#4102](https://github.com/gastownhall/beads/issues/4102), [#5273](https://github.com/gastownhall/beads/issues/5273), [#3239](https://github.com/gastownhall/beads/issues/3239). Changes merged after 1.3.1 cut the count for some commands (for example `bd ready --limit 1` from about 48 to about 21 statements), but it stays in the tens, so at this RTT the delay stays in seconds.

With a server in the same region (RTT of a few milliseconds) the same count adds a fraction of a second.

#### Pattern: run bd next to the database

Keep the workspace (a clone of the repository with its `.beads/` directory, configured for server mode) on the machine that runs the Dolt server, and start `bd` there. The SQL round-trips then happen over loopback, and only one round-trip (the SSH exchange) crosses the slow link. With SSH connection sharing, the connection is opened once and later commands reuse it, so each call costs about two RTT plus the local runtime.

A minimal wrapper, saved on the client as `bdr`:

```bash
#!/usr/bin/env bash
# bdr: run bd on the machine that hosts the Dolt server.
set -euo pipefail
HOST=dolt-host.example.com        # the machine running dolt sql-server
REPO='~/work/myproject'           # the workspace on that machine
args=$(printf ' %q' "$@")         # keeps quoting intact across the remote shell
exec ssh -o ControlMaster=auto \
         -o ControlPath="$HOME/.ssh/cm-%C" \
         -o ControlPersist=10m \
         "$HOST" "bd -C $REPO --actor $(printf %q "$USER")$args"
```

```bash
bdr ready --json
bdr create "Fix the thing" --stdin < description.md
```

Details that matter:

- `-C` selects the workspace on the remote side, like `git -C`; the working directory of the client is not used.
- `--actor` is passed explicitly. Otherwise bd resolves the actor from `BEADS_ACTOR`, `git user.name` or `$USER` on the remote machine, and the audit trail records that identity.
- Standard input is forwarded by SSH, so `--stdin` and `--body-file -` work. A path such as `--body-file notes.md` is read on the remote machine, not on the client; send the content through stdin instead.

#### Limits

- Commands that need a terminal (`bd edit`, `bd create-form`, `bd show --watch`, confirmation prompts) need `ssh -t`, or should run with the local bd.
- Commands tied to the machine (`bd init`, `bd config`, `bd dolt ...`, `bd backup`, `bd restore`, `bd bootstrap`) act on the remote workspace when sent through the wrapper. Run them locally if you mean the local one.
- Hooks fire where bd runs, that is on the Dolt host, and only if the workspace there has them installed.
- The workspace on the Dolt host is a second clone. It has to be in server mode against the same database (check `.beads/metadata.json`), and it has to be kept up to date by git like any other clone.
- Everyone who uses the wrapper needs SSH access to that host. Treat it as shell access.
- One extra moving part: if the SSH master connection drops, the next call pays a new handshake.

#### Alternatives

| Option | What it gives | What it does not |
|---|---|---|
| Proxied-server mode (`bd init --proxied-server`, experimental) | A local proxy process owns the server lifecycle; can front an external Dolt | Reducing the statements per command is not its documented goal; no measurement against a remote server found |
| Embedded mode plus Dolt remote sync (see [Sync Concepts](/core-concepts/sync-concepts), [Bucket Federation](/multi-agent/bucket-federation)) | Every command is local and fast; works offline | One writer at a time; replicas diverge between syncs; schema migrations must be applied by one clone (see [Upgrading](/getting-started/upgrading)) |
| Same-region server (VPN or tunnel to a nearby host) | No wrapper | Only helps when a near host exists |
| Wait for upstream fixes | | Open work: [#6114](https://github.com/gastownhall/beads/issues/6114), [#5965](https://github.com/gastownhall/beads/issues/5965), [#5273](https://github.com/gastownhall/beads/issues/5273) |

If commands are slow only against a remote server and fast with the server on the same machine, round-trip latency is the cause; `bd` on the server host is the fastest fix available today.
