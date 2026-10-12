Closes #7517

Refs #6114, #4102, #5273, #3239. This documents a workaround; it does not fix the latency.

## Problem

`docs/architecture/dolt.md` explains server mode but says nothing about its cost against a Dolt server on another machine. Every bd command runs its SQL statements one after another, so the network round-trip time (RTT) is paid once per statement. #6114 counted 44 to 48 awaited round-trips for the cheapest commands on 1.2.1; #4102, #5273 and #3239 report the same at 0.1 to 0.25 s RTT. A reader who hits this has nothing in the docs that names the cause or offers a way out.

## Change (docs only)

One new subsection in `docs/architecture/dolt.md`, between "Server Mode" and "Maintenance" (+70 lines, no code, no `docs.json` change):

- What it costs: measured numbers with versions and RTT, and why the cost follows RTT and not the size of the result.
- Pattern "run bd next to the database": a minimal SSH wrapper with connection sharing, and the details that matter (`bd` on the PATH of a non-interactive shell, `-C`, explicit `--actor`, stdin forwarding, file arguments read on the remote side).
- Limits: commands that need a terminal, machine-bound commands, hooks, a second clone on the Dolt host, SSH access is shell access.
- Alternatives table: proxied-server mode, embedded mode plus Dolt remote sync, a same-region server, waiting for the open work in #6114, #5965, #5273.

## Evidence it works

Server in another region (RTT 0.24 s through an SSH port forward), `bd show <id>`, median of 3 runs per cell, same issue every time:

| Where bd runs | 1.3.0 | 1.3.1 | `main` (b630f96) |
|---|---|---|---|
| Laptop, Dolt server remote | 10.2 s | 10.3 s | 10.8 s |
| Dolt host, via reused SSH connection (the wrapper from the text) | 0.73 s | - | - |
| Dolt host, local shell | 0.17 s | - | - |

The wrapper was run as written: `ready --json`, `list`, `show`, and `create --stdin` (body returned byte for byte, quotes and `$HOME` intact) work, and `--body-file` with a relative path is read on the remote side, as the text says. The first call without an SSH master took 5.3 s. The statement counts quoted in the text (24 for `bd list --limit 1`, 40 for `bd show` on `main`) come from a delaying TCP proxy in front of a local `dolt sql-server --loglevel debug`, counting "Starting query" lines. More detail in [my comment on #6114](https://github.com/gastownhall/beads/issues/6114#issuecomment-6128075017).

Checks run on this branch (docs-only validation from `engdocs/TESTING.md`):

- `git diff --check`: clean.
- `make check-docs` (Bazel, 4 min 40 s on a cold cache, macOS arm64): exit 0. `//test/docsync:docsync_test` and `//scripts/repochecks:doc_freshness_test` pass, then `scripts/check-doc-flags.sh` against the pinned v1.2.2 prints `PASSED`.
- `make check-docs-go`: exit 0.

## Checklist

- [x] The why is documented: in the linked issue and in this description.
- [x] `make check-docs` (the docs lane in `engdocs/TESTING.md`) passes locally; `make test` is not affected, no code changed.
- [x] User-facing behavior changes are reflected in `docs/`: this PR is the docs change.
- [x] No invariant or rule changed, so no `AGENTS.md` update.

AI disclosure: the text was drafted by Claude Code agents under my direction; the timings were measured on my own setup.

_claude-code-claude-opus-5-5-unknown-reasoning on behalf of serejaris_
