# Черновик docs-раздела для gastownhall/beads: bd при удалённом Dolt-сервере

Статус: черновик, наружу не отправлялся. Описание и вердикт — [beads-docs-remote-dolt-2026-10-12.md](beads-docs-remote-dolt-2026-10-12.md). Бид `cos-e4x.2`.

Куда класть в дереве апстрима: `docs/architecture/dolt.md`, новый подраздел `### Remote server: latency and running bd next to the database` сразу после подраздела «Server Mode (Multi-Writer / Orchestrator)» и перед `## Maintenance`. Запасной вариант, если мейнтейнеры не хотят растить dolt.md: отдельная страница `docs/architecture/remote-server.md` плюс строка в `docs/docs.json` (группа Architecture). Ссылка-указатель из `docs/reference/troubleshooting.md` § Performance Issues (подраздел «Slow commands against a remote server», 3 строки со ссылкой на якорь).

Всё ниже границы — текст для PR (английский).

---

### Remote server: latency and running bd next to the database

Server mode works against a Dolt server on another machine (`BEADS_DOLT_SERVER_HOST`, or `dolt.host` in `.beads/config.yaml`). Every bd command is a short-lived process that opens its own connections and then runs a series of SQL statements one after another. Each statement waits for the previous answer, so the network round-trip time (RTT) is paid once per statement, not once per command.

#### What it costs

Measured with bd 1.3.0 against a Dolt server on another continent (RTT 0.24 s through an SSH port forward), median of 3 runs per row, same issue every time. bd 1.3.1 gave the same numbers within 0.1 s; a build of `main` (b630f96, 2026-10-12) took 10.8 s for `bd show`:

| Where bd runs | `bd show <id>` |
|---|---|
| On the laptop, Dolt server remote | 10.2 s |
| On the Dolt host, started over a reused SSH connection | 0.73 s |
| On the Dolt host, in a local shell | 0.17 s |

The cost scales with RTT, not with the size of the result. A packet-counted accounting in [#6114](https://github.com/gastownhall/beads/issues/6114) found 44 to 48 awaited round-trips for even the cheapest commands on 1.2.1 (`bd close`: 150). At 0.24 s that is 10 s or more before the command does any work of its own. Other reports with 0.1 to 0.25 s RTT: [#4102](https://github.com/gastownhall/beads/issues/4102), [#5273](https://github.com/gastownhall/beads/issues/5273), [#3239](https://github.com/gastownhall/beads/issues/3239). Merged changes cut the count for some commands (for example [#6731](https://github.com/gastownhall/beads/pull/6731) for `bd ready`), but in our run on `main` the counts were still in the tens (`bd list --limit 1`: 24 statements, `bd show`: 40), so at this RTT the delay stays in seconds.

With a server in the same region (RTT of a few milliseconds) the same count adds a fraction of a second.

#### Pattern: run bd next to the database

Keep the workspace (a clone of the repository with its `.beads/` directory, configured for server mode) on the machine that runs the Dolt server, and start `bd` there. The SQL round-trips then happen over loopback, and only one round-trip (the SSH exchange) crosses the slow link. With SSH connection sharing, the connection is opened once and later commands reuse it, so each call costs about three RTT plus the local runtime (0.73 s at RTT 0.24 s). The first call after the master expires pays the full SSH handshake, several seconds over a jump host in our test.

A minimal wrapper, saved on the client as `bdr`:

```bash
#!/usr/bin/env bash
# bdr: run bd on the machine that hosts the Dolt server.
set -euo pipefail
HOST=dolt-host.example.com        # the machine running dolt sql-server (an ssh alias works)
REPO='~/work/myproject'           # the workspace on that machine; ~ is expanded by the remote shell
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

- `bd` has to be on the PATH of a non-interactive ssh shell on the remote machine (`ssh HOST 'command -v bd'`). If it lives in `~/.local/bin` and that is only added by `.bashrc`, use the full path in the wrapper.
- `-C` selects the workspace on the remote side, like `git -C`; the working directory of the client is not used.
- `--actor` is passed explicitly. Otherwise bd resolves the actor from `BEADS_ACTOR`, `git user.name` or `$USER` on the remote machine, and the audit trail records that identity.
- Standard input is forwarded by SSH, so `--stdin` and `--body-file -` work. A path such as `--body-file notes.md` is read on the remote machine, not on the client; send the content through stdin instead.
- The wrapper reads the caller's standard input. Inside a `while read` loop it swallows the rest of the loop's input after the first call; give each call its own, for example `bdr list < /dev/null`.

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
| Proxied-server mode (`bd init --proxied-server`, experimental) | A local proxy process owns the server lifecycle; `--proxied-server-external-host` and `--proxied-server-external-port` point it at an externally managed Dolt server | Reducing the statements per command is not its documented goal, and we did not measure it against a remote server |
| Embedded mode plus Dolt remote sync (see [Sync Concepts](/core-concepts/sync-concepts), [Bucket Federation](/multi-agent/bucket-federation)) | Every command is local and fast; works offline | One writer at a time; replicas diverge between syncs; schema migrations must be applied by one clone (see [Upgrading](/getting-started/upgrading)) |
| Same-region server (VPN or tunnel to a nearby host) | No wrapper | Only helps when a near host exists |
| Wait for upstream fixes | Nothing to set up | Open work: [#6114](https://github.com/gastownhall/beads/issues/6114), [#5965](https://github.com/gastownhall/beads/issues/5965), [#5273](https://github.com/gastownhall/beads/issues/5273) |

If commands are slow only against a remote server and fast with the server on the same machine, round-trip latency is the cause; `bd` on the server host is the fastest fix available today.

---

## Critique: actionability + remeasure

Дата: 2026-10-12. Ось: actionability. Вердикт: **FIX** внесён, после него **PASS** (текст читается без нашего контекста, пример воспроизводим). Upstream actions: `0`.

Пример `bdr` прогнан буквально (HOST подставлен ssh-алиасом, REPO — workspace того же графа на Dolt-хосте), bd 1.3.0 на удалённой стороне. `ready --json`, `list --limit 3`, `show`: работают, медиана тёплого вызова 0,73 с (3 прогона, RTT 0,24 с; в черновике было 0,85). Первый вызов без мастера: 5,3 с (handshake через jump-хост). `create "docs wrapper probe" --stdin < file` создал бид, тело вернулось побайтно (кавычки, `$HOME`, обратные апострофы), автор `ris`, `--body-file -` тоже работает, `--body-file relative.md` падает с `open ...: no such file or directory` на удалённой стороне, как и написано. Пробные бидзы закрыты, графа не засоряют. `-C`, `--actor`, `--stdin`, `create-form`, `edit`, `bootstrap`, `backup`, `restore` есть в `--help` 1.3.0.

Поправлено в черновике: таблица (10,2 с / 0,73 с, 1.3.1 в пределах 0,1 с), «about two RTT» → «about three RTT» + цена первого вызова, требование `bd` в PATH нон-интерактивного ssh-шелла, пояснение про `~` в `REPO`, ловушка со stdin (в `while read` обёртка съедает остаток ввода: проверено, цикл из трёх элементов дал один проход; лечится `< /dev/null`), пустая ячейка в таблице альтернатив.

Остаётся: (а) строка про Proxied-server «can front an external Dolt»: в `bd init --help` (1.3.0 и main) режим описан как per-workspace прокси + дочерний dolt, `listener.host` только numeric loopback, remotesapi запрещён; формулировка про «внешний Dolt» с этим не сходится, снять или проверить по docs апстрима; (б) счёт «~48 → ~21 выражений для `ready --limit 1`» против нашего 44 на main (см. заметку по #6114); (в) цифры #6114 про 44-48 round-trips и 150 для `bd close` на 1.2.1 здесь не перепроверялись (ось фактологии).
