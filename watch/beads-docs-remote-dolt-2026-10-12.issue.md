### Before you continue

- [x] I searched existing issues and pull requests and did not find a duplicate.

### Motivation

The docs say nothing about what bd costs against a Dolt server on another machine. Every bd command is a short-lived process that runs its SQL statements one after another, so the network round-trip time (RTT) is paid once per statement. Reports of this: #6114 (44 to 48 awaited round-trips even for the cheapest commands on 1.2.1), #4102, #5273, #3239.

Measured with bd 1.3.0 against a Dolt server on another continent (RTT 0.24 s through an SSH port forward), `bd show <id>`, median of 3 runs, same issue every time:

| Where bd runs | `bd show <id>` |
|---|---|
| Laptop, Dolt server remote | 10.2 s |
| On the Dolt host, started over a reused SSH connection | 0.73 s |
| On the Dolt host, in a local shell | 0.17 s |

bd 1.3.1 gave the same numbers within 0.1 s; a build of `main` (b630f96) took 10.8 s. Details and statement counts are in [my comment on #6114](https://github.com/gastownhall/beads/issues/6114#issuecomment-6128075017). People who hit this today have no documented way to tell that RTT is the cause, and no documented workaround.

### Proposed change

A docs-only subsection in `docs/architecture/dolt.md`, after "Server Mode", that covers:

- why the cost scales with RTT and not with the size of the result, and how to recognise it (slow only against a remote server, fast with the server on the same machine);
- the measured numbers above, with versions and RTT;
- the pattern "run bd next to the database": a minimal SSH wrapper with connection sharing, and the details that matter (`bd` on the PATH of a non-interactive shell, `-C`, explicit `--actor`, stdin forwarding, file arguments read on the remote side);
- limits (commands that need a terminal, machine-bound commands, hooks, a second clone on the Dolt host, SSH access is shell access);
- a table of alternatives (proxied-server mode, embedded mode plus Dolt remote sync, a same-region server, waiting for the open perf work in #6114, #5965, #5273).

The patch is ready: https://github.com/serejaris/beads/commit/ca52db3ed2ba32e7f8185f7fb9789fd7c84716f2 (70 lines, one file).

### Impact

Users and agents that run bd from a laptop or CI against a shared Dolt server in another region. No change for anyone else. No code, `--json` output, or integration touched.

### Risk and compatibility

None for behavior: docs only, no schema, wire, or `--json` changes. The numbers are tied to bd 1.3.0/1.3.1 and one RTT, and the text says so; as the perf work in #6114 lands they will age, and the section tells readers how to re-measure. Rollback is a revert of the one commit.

### Verification plan

`make check-docs` (docsync and doc freshness tests, then the CLI flag check). The wrapper in the text was run as written against a real remote server: `ready --json`, `list`, `show`, and `create --stdin` work, and `--body-file` with a relative path is read on the remote machine, as the text says. The timings can be reproduced with `time bd show <id>` in each of the three setups.

### Alternatives considered

- Wait for the open perf work (#6114, #5965, #5273). Those reduce the statement count but, at this RTT, the delay stays in seconds (on `main`, `bd show` is still 40 statements in my run).
- Proxied-server mode: its documented goal is not fewer statements per command, and I did not measure it against a remote server; the section says so.
- Embedded mode plus Dolt remote sync: fast and offline, but one writer at a time.
- Put the text in `docs/reference/troubleshooting.md` instead: it is a pattern with a wrapper and limits, which fits the architecture page better; a pointer from troubleshooting can follow.

### Estimated size

S (under 200 lines)

### Intent

I am opening or will open a PR

### AI assistance

Yes, mostly

### Accountability

- [x] A human has read this issue and stands behind it.

AI disclosure: the text was drafted by Claude Code agents under my direction; the timings were measured on my own setup.

_claude-code-unknown-model-unknown-reasoning on behalf of serejaris_
