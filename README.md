# Mise for Mere Mortals

Every project wants its own versions. The
tutorial assumes Node 22, the older project
was built with Node 20, a script at work
only runs on Node 18 — and each language
traditionally brings its own version
manager, with its own commands and its own
config file, until the machine is a drawer
of single-purpose tools. **Mise** (short
for *mise en place*, the cook's discipline
of having every ingredient prepared and in
its place) is one program that installs
and pins **every** tool a project needs —
languages, runtimes, and small utilities —
in one small config file per project, so
that moving into a folder quietly puts the
right versions in charge.

This guide explains how that works: how
mise is put together, how to install it
and have it fetch your first tool, how a
project pins its versions in a `mise.toml`
you can commit and share, how mise decides
which version applies when several files
speak at once, and how the same config
file can carry a project's tasks and
environment variables. It finishes, as
this series does, with the neighbours, the
limits, and the troubleshooting — because
a tool that manages your whole toolchain
deserves its edges stated plainly.

The rule that governs everything here is
stated in chapter 1: **pin only tools you
would install by hand, from the sources
the tool itself uses — and read a
project's config before you let mise trust
it.** A version manager downloads and runs
other people's binaries, and a project
config can contain commands; mise is
honest machinery for both, and this guide
treats them with the care they ask for.

The lab behind this guide ran the real
thing: **mise 2026.10.21**, the official
release binary, on a small Linux machine.
Against five small fixture projects of the
guide's own invention, it installed Node
22 as a global default, added Node 20
beside it, fetched the ripgrep search tool
through a second backend, pinned the two
projects to different Node versions and
watched `node --version` change its answer
as it moved between folders — twice, by
two different mechanisms. It ran project
tasks (including one defined in a file,
and one with a dependency), set a project
environment variable and found it inside a
real command, honoured a legacy
`.tool-versions` file from the older asdf
manager, measured how far behind an
exactly pinned project had fallen, let a
missing version install itself on first
use, and was refused — correctly — by an
untrusted config file until it approved
it.

What the lab could not do, it did not
fake. It used one machine and one
operating system, so the other install
routes, the other shells' activation
lines, the tools mise compiles from source
rather than downloads, the backends beyond
the two exercised, and Windows behaviour
are taught from the project's
documentation and labelled as such
wherever they appear. `criteria.md` keeps
the full ledger of what was run and what
was only read.

## What this guide covers

- [**What mise is**](what-mise-is.md) —
  the version-soup problem, the one-file
  answer, and the rule about trusting
  tools and configs, stated before
  anything depends on it
- [**The moving parts**](the-moving-parts.md) —
  config files, backends, the installs
  shelf, and the three ways a pinned tool
  gets run
- [**Installing mise**](installing-mise.md) —
  the release archive (the lab's route),
  the installer script and packages, and
  the folders mise keeps, via `mise doctor`
- [**Your first tool**](your-first-tool.md) —
  Node installed and set as the global
  default with one command, then run
  through `mise exec`
- [**Pinning versions per project**](pinning-versions.md) —
  `mise use` in a project, the `mise.toml`
  it writes, the `mise trust` step, and
  the legacy `.tool-versions` file
- [**How mise decides**](how-mise-decides.md) —
  nearest file wins, files combine per
  tool, subfolders inherit — and `current`,
  `config` and `which` to see it work
- [**More tools and backends**](more-tools-and-backends.md) —
  a second Node on the shelf, ripgrep
  through the aqua registry, short names,
  and what the other backends are for
- [**Tasks**](tasks.md) — a project's
  common commands, named in its config or
  as files in `mise-tasks/`, run with
  `mise run`
- [**Environments**](environments.md) —
  the `[env]` table: variables a project
  lends to its tools, present inside the
  work and absent outside it
- [**Shims, activation, and exec**](shims-activation-and-exec.md) —
  the three ways `node` becomes the pinned
  Node, each demonstrated by walking
  between projects — plus auto-install
- [**Keeping tools current**](keeping-tools-current.md) —
  `outdated` within your ranges, the
  `--bump` view beyond them, upgrading,
  and tidying the shelf
- [**Recipes**](recipes.md) — try a tool
  for the weekend, onboard a fresh clone,
  match CI to your laptop, pin a project
  instead of downgrading a machine
- [**The neighbours**](the-neighbours.md) —
  the single-language managers, asdf,
  Volta and proto, devcontainers, Nix and
  direnv: what each does better
- [**Limits**](limits.md) — tools that
  compile, versions that are not
  isolation, the supply chain a pin
  trusts, and a config file that is code
  near your shell
- [**Troubleshooting**](troubleshooting.md) —
  the wrong version, the untrusted
  config, the missing task, the command
  that is not found, and the traps this
  guide's lab fell into so you don't have to
- [**Glossary**](glossary.md) — the terms,
  collected in one place

## What it doesn't cover

Installing tools you would not install by
hand — a version manager is a download
pipe into your machine, and chapter 14
explains why its sources matter.
Isolating projects from each other or
from the machine: that is the container
tools' work, pointed at in chapter 13,
not impersonated here. Writing backends
or plugins of your own, administering
mise for a whole organisation (mirrored
registries, policy, fleets), and deep
dives into any single language's own
manager are likewise out; this guide
stays with one person, their projects,
and their machine.

## How to use this guide

Read chapter 1 for the problem and the
rule, and chapter 2 before installing
anything — knowing which part holds what
turns later confusion into a named
problem. Chapters 3 and 4 belong in one
sitting: install mise, then make it fetch
Node. Chapter 5 is the heart of the book
— pin real projects of your own as you
read it, and do not skip the trust step;
it is short, and it is the difference
between a config file and a surprise.
Chapter 10 repays care: pick *one* of
shims or activation to start with, prove
it with the walk between two folders, and
only then read the other. The lab's
transcripts live in `mise-lab/transcripts/`;
its fixture projects are `mise-lab/projects/`;
its notes are `examples/lab-notes.txt` and
its distilled commands `examples/commands.txt`.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [Python for Mere Mortals](https://paulsullivan33-dev.github.io/python-for-mere-mortals/)
- [Node.js for Mere Mortals](https://paulsullivan33-dev.github.io/nodejs-for-mere-mortals/)
- [Go for Mere Mortals](https://paulsullivan33-dev.github.io/go-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/mise-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/mise-for-mere-mortals/blob/main/LICENSE-CODE).
