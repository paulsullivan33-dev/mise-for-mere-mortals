# The Moving Parts

Mise looks like one command, but it makes
fewer decisions in one place than you
would expect. Knowing which part holds
what is the difference between "mise is
broken" and "this folder's config asked
for a version that was never installed" —
a distinction chapter 15 leans on.

## The command itself

`mise` is a single program — one binary,
no daemon, nothing running in the
background. Every command in this guide
is mise waking up, reading the config
files that apply where you stand, doing
the job, and exiting. When versions
change as you move between folders
(chapter 5), no watcher is following you:
the change is worked out fresh each time
a tool is invoked, by one of the three
mechanisms in chapter 10.

## Config files: what is pinned where

A **config file** is where versions are
declared. There are two kinds, and both
are plain text you can read and edit:

- **The global config**,
  `~/.config/mise/config.toml`, holds
  your defaults — the versions that
  apply when no project says otherwise.
  Chapter 4's first `mise use --global`
  writes it.
- **A project's `mise.toml`** sits in a
  project folder and overrides the
  global config for anything done inside
  that folder — including its
  subfolders (chapter 6 shows a pin
  still holding two levels down).

Both use the same shape: a `[tools]`
table naming tools and the versions
wanted, plus optional `[env]` entries
(chapter 9) and `[tasks]` (chapter 8).
Chapter 6 covers exactly how mise
combines the files when several apply.

## Backends: where tools come from

Mise does not contain Node, or Python, or
anything else it manages. For each tool
it asks a **backend** — a recipe for
finding, downloading and unpacking that
tool's releases. The `mise doctor` output
in transcript t00 lists the ones baked
into this build; the ones this guide uses
or names are:

| Backend | Where the tool comes from |
|---|---|
| **core** | Built-in recipes for the big languages and runtimes (Node, Python, Go, Ruby, …), fetched from each project's own release channels |
| **aqua** | The community **aqua registry**: thousands of CLI tools, each fetched from its own project's release page (chapter 7 installs ripgrep this way) |
| **github** | A named GitHub repository's releases, directly |
| **asdf** | Plugins written for **asdf**, the older version manager mise succeeds — a long tail of community recipes |
| **cargo, npm, pypi, gem, go** | The tool republished through that language's own package system |
| **ubi** | A generic installer for tools that ship as a lone binary on a releases page |

The backend matters because it is the
supply chain (chapter 1's rule): the
core Node recipe downloads from
nodejs.org, the aqua recipe for ripgrep
downloads ripgrep's own GitHub release.
`mise which` (chapter 6) shows the real
file any tool resolved to, so the chain
is never a matter of faith.

## The installs directory

Downloaded tools live side by side under
**`~/.local/share/mise/installs/`** — one
folder per tool, one subfolder per
version. Node 20 and Node 22 coexist
there happily; a few megabytes of disk
buys never having to uninstall one
version to try another. Small marker
folders (`20`, `20.20`, `latest`) sit
alongside the real version folders so
mise can resolve a partial request like
"20" to the full version installed.
`mise ls` (chapter 4) is the tidy view of
the same shelf, and chapter 11 covers
removing versions you no longer need.

## Shims, activation, and exec

Three ways connect "the config says
Node 20" to "typing `node` runs Node
20", and chapter 10 gives each its due:

- **Shims** — a folder of tiny stand-in
  programs, one per tool, put on your
  `PATH` once. A shim asks mise "what
  applies here?" and hands over to the
  real binary. No shell setup beyond the
  one `PATH` line.
- **Activation** — a line in your shell's
  startup file that lets mise adjust the
  environment as you change folders.
  The closest to the classic managers'
  feel.
- **Exec** — no setup at all: you ask
  mise to run one command (`mise exec --
  node --version`), and it sets up just
  that process. Scripts and CI servers
  live here.

## Tasks and environment, riding along

The same config file can carry two
passengers. **Tasks** are named commands
— `mise run report` — defined per project
(chapter 8), including *file tasks*:
executable scripts in a `mise-tasks/`
folder. **Environment entries** are
variables mise sets whenever it works in
the project (chapter 9). Both are why a
config file can *do* things — and why
mise asks you to trust one it did not
write itself (chapter 5).

## Who holds what, when it breaks

- The **config files** hold the intent.
  If the wrong version runs, the answer
  is in which files apply here — `mise
  config` lists them (chapter 6).
- The **installs directory** holds the
  reality. A pinned version that was
  never installed is an intent without a
  tool; chapter 10 shows mise fetching
  it on first use, and chapter 15 what
  to check when it doesn't.
- The **backend** holds the supply
  chain. A tool that will not install is
  usually its backend's source being
  unreachable or the version not
  existing — `mise ls-remote` (chapter
  11) separates those two.
- **Trust** holds the safety line.
  Configs with tasks, env or hooks act
  only after `mise trust` (chapter 5).
