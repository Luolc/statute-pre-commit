# statute-pre-commit

> **This is an automated mirror and does not accept manual contributions.**
> The statute version pin and the `v<version>` tags here are written only by
> statute's release workflow in
> [Luolc/agent-constitution](https://github.com/Luolc/agent-constitution).
> To change statute itself or the version this hook installs, make the change
> there.

A [pre-commit](https://pre-commit.com/) hook for
[statute](https://github.com/Luolc/agent-constitution), the command that
compiles a constitution and the rules a repository chooses into the
instruction files coding agents read.

Distributed as a standalone repository to enable installing statute via
prebuilt wheels from PyPI: this hook needs no Rust toolchain and compiles
nothing.

## Usage

```yaml
repos:
  - repo: https://github.com/Luolc/statute-pre-commit
    rev: <tag>  # see the tags of this repository
    hooks:
      - id: statute-compile
```

The hook runs `statute compile --check` at the repository root. It writes
nothing: it fails, naming each file, when the committed `AGENTS.md` and the
files statute writes beside it differ from a fresh compile of `statute.toml`
and `statute.lock`. To write them, run `statute compile` yourself.

`rev` is a tag of *this* repository, and each tag pins exactly one statute
version: `v0.1.18` installs `statute==0.1.18`. The tags are written by
statute's own release workflow, so they track upstream releases one for one.
Upstream tags its releases `statute-v<version>`; the tags here are plain
`v<version>`.

## Keep `rev` in step with `statute.lock`

`statute.lock` records the statute version that wrote it
(`statute = "0.1.18"`), and `statute compile --check` refuses a lock written
by any other version, saying `... was written by statute <that version>, and
this is statute <this version>`. So `rev: v<X>` must match the lock's
`statute = "<X>"`: upgrade statute and the lock together, and change `rev` in
the same commit.

## When the hook runs

The hook compiles the whole repository, so it takes no file names; `files`
only decides which commits run it. The default covers `statute.toml`,
`statute.lock`, `AGENTS.md`, `.statute/`, `.agents/`, `.claude/` and
`.pre-commit-config.yaml` (so that a commit changing `rev` runs it too). If
your repository keeps statute's inputs or outputs elsewhere, override `files`
in your own configuration:

```yaml
hooks:
  - id: statute-compile
    files: ^(statute\.(toml|lock)$|AGENTS\.md$|\.statute/|\.agents/|\.claude/|\.pre-commit-config\.yaml$|my/other/path/)
```

## No Windows wheels

statute ships prebuilt wheels for Linux (x86_64 and aarch64, glibc and musl)
and macOS (x86_64 and arm64). There is no Windows build, so on Windows
`pip install statute` finds no wheel and this hook fails to install. There is
no source-building hook upstream either; on Windows, install statute another
way (see [its README](https://github.com/Luolc/agent-constitution#readme))
and run it from a `language: system` hook in a `repo: local` block.

## This repository is generated

Everything here except the `statute==` pin in `pyproject.toml` is
hand-written, and that pin is rewritten by statute's release workflow on every
release. Do not edit it by hand; the next release overwrites it.

Issues and pull requests belong upstream, at
[Luolc/agent-constitution](https://github.com/Luolc/agent-constitution).

## License

Apache License, Version 2.0 — the same as statute's code. See
[`LICENSE`](LICENSE).
