# Agent rules (this repository)

**Repository language: English.** Everything that lands here — README, YAML
comments, commit messages — is written in English, whatever language the
request came in. This repository is read by pre-commit users in any ecosystem,
so the Chinese default that applies to some of the owner's other repositories
does not apply here.

Cross-repository agent rules live in `~/.agents/AGENTS.md` and still hold; this
file only states what is specific to this repository.

## What this repository is for

One thing: letting `pre-commit` install statute from a prebuilt PyPI wheel
instead of compiling it. pre-commit has 21 languages and none of them is
"download a prebuilt binary"; `language: python` is the way in, because
pre-commit is itself written in Python. That language requires `pip install .`
to work at the root of the hook repository, which means a `pyproject.toml` at
the root, and upstream's root `pyproject.toml` is the uv workspace of its own
tools, not statute. Hence this repository.

It has no build, no tests and no release process of its own.

## The version pin is generated, not authored here

The `statute==<version>` line in `pyproject.toml`, and the `v<version>` tags
of this repository, are written by the `mirror` job of the release workflow in
[Luolc/agent-constitution](https://github.com/Luolc/agent-constitution)
(`.github/workflows/release.yml`), after that version's wheels are live on
PyPI. Upstream tags a release `statute-v<version>`; the tag here is
`v<version>`. A hand edit is overwritten by the next upstream release and
leaves no trace of why it was made.

So: **do not edit that line, and do not tag this repository by hand.** Change
the upstream workflow instead, and let a release write the result. The one
exception was the first tag, `v0.1.18`, set by hand once before the upstream
job existed.

Keep the line exactly `dependencies = ["statute==<version>"]`: the upstream
job rewrites it with `sed`, which matches that shape.

Everything else here (`.pre-commit-hooks.yaml`, `README.md`, `LICENSE`, this
file) is hand-written and is never touched by the workflow. Changing the hook
definition — its `entry`, its `files` pattern, a new id — is an ordinary
change to this repository.

## The hook only checks

`entry` is `statute compile --check`, which writes nothing. Do not turn it
into `statute compile`: a hook that writes `AGENTS.md` would hide a stale
compile instead of reporting it.
