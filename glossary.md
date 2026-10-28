# Glossary

**Activation** — the arrangement where a
line in your shell's startup file lets
mise adjust `PATH` and environment
variables as you change folders
(chapter 10).

**asdf** — the older, plugin-based
version manager mise succeeds; its
`.tool-versions` files and plugins still
work with mise (chapters 5 and 13).

**aqua** — a community registry of CLI
tools and the backend that fetches them
from their projects' own release pages
(chapter 7).

**Auto-install** — mise's habit of
downloading a pinned-but-missing version
the first time it is asked for, on by
default; a setting can turn it off
(chapter 10).

**Backend** — the recipe mise uses to
find, download and unpack a given tool:
core, aqua, github, asdf and others
(chapter 2).

**Bump** — moving a pin beyond the range
it originally allowed (`mise outdated
--bump` shows how far that is,
chapter 11).

**Config file** — where pins live: the
global `~/.config/mise/config.toml`, or a
project's `mise.toml` (chapter 2).

**Core tools** — the languages and
runtimes mise knows natively, fetched
from each project's own release channel
(chapter 2).

**Exec** — `mise exec -- <command>`:
running one command inside the toolset
that applies here, with no shell setup
(chapters 4 and 10).

**File task** — a task defined by an
executable script in a project's
`mise-tasks/` folder instead of in the
config file (chapter 8).

**Global config** — your machine-wide
defaults, applying wherever no project
pin overrides them (chapters 2 and 4).

**Installs directory** —
`~/.local/share/mise/installs`, the shelf
where every downloaded version lives side
by side (chapter 2).

**Pin** — a tool-and-version request
recorded in a config file: `node = "20"`
(chapter 5).

**Registry** — the catalogue behind a
backend: aqua's tool list, or mise's
short names for popular tools
(chapter 7).

**Resolve** — turning a request like
"20" into a full version, 20.20.2, by
matching it against the releases a
backend knows (chapter 4).

**Shim** — a tiny stand-in program with
a tool's name that asks mise what applies
in the current folder and hands over to
the real binary (chapter 10).

**Short name** — a friendly alias mise
knows (`node`, `ripgrep`) that selects a
recommended backend for a tool
(chapter 7).

**Task** — a named command a project
defines in its config, run with
`mise run <name>` (chapter 8).

**`.tool-versions`** — the older asdf
config format, one `tool version` per
line; mise reads it as-is (chapter 5).

**Trust** — your explicit approval of a
project config mise did not write:
`mise trust` lets its tasks, environment
and hooks take effect (chapter 5).

**`mise.toml`** — the per-project config
file: tools, environment and tasks in one
place (chapters 2 and 5).
