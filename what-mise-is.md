# What Mise Is

Every programmer's machine collects
versions. The tutorial wants Node 22, the
project at work was built with Node 20,
the old script only runs on Node 18, and
the system package manager offers one
Node, take it or leave it. So the machine
accumulates **version managers**: one for
Node, another for Python, a third for
Ruby, each with its own commands, its own
config file, its own way of deciding what
runs. **Mise** (pronounced *meez*, short
for *mise en place* — the cook's habit of
having every ingredient prepared and in
its place before cooking starts) replaces
that drawer of single-purpose tools with
one program that installs and pins **every**
tool a project needs — languages, runtimes,
and plain command-line tools — in one
config file per project.

The pin is the point. A project carries a
small file, **`mise.toml`**, that says:
*this project uses Node 20 and version
14 of the search tool ripgrep.* Anyone who
opens the project — a colleague, a CI
server, you in eighteen months — can read
the file and get exactly those versions.
Move into the folder and the right
versions are simply what runs; move out
and they stop applying. Chapter 5 makes a
pair of projects prove it.

## Why one tool instead of many

The single-language managers (chapter 13)
are good tools, and mise is frank about
standing on their shoulders — it even
reads their config files (chapter 5's
legacy fixture). What one tool buys you:

- **One syntax to learn.** Node, Go,
  Python, and a thousand smaller tools
  are all pinned the same way, in the
  same file, with the same commands.
- **One file to commit.** The project's
  whole toolchain — runtimes *and* the
  utilities its scripts call — travels
  with the code, instead of living in a
  wiki page nobody updates.
- **Room to grow.** The same file can
  also carry the project's environment
  variables (chapter 9) and its common
  commands as **tasks** (chapter 8), so
  "how do I run this project?" has one
  answer instead of three documents.

## The rule, stated before everything else

A version manager is a program that
downloads other programs and runs them,
and a project's config file can contain
commands (tasks) and settings that take
effect when you enter the folder. That is
exactly as powerful as it sounds, which
is why this guide has one rule, and it is
not fine print:

> **Pin only tools you would install by
> hand, from the sources the tool itself
> uses — and read a project's config
> before you let mise trust it.**

Downloading a runtime from its project's
own release page: that is what you would
do by hand, and it is what mise does
(chapter 2 names the sources). Cloning a
stranger's repository and letting its
`mise.toml` run tasks on your machine
unread: no — and mise agrees, which is
why it refuses to use parts of a config
it did not write until you approve them
with `mise trust` (chapter 5, transcript
t11). This guide's lab obeyed the rule:
every tool it installed came from the
tool's own official release channel, and
every fixture project is the lab's own
invention.

## A familiar shape

If you have met the tools mise replaces,
the shape is familiar: a small set of
commands (`use`, `install`, `ls`,
`current`, `run`) that feel the same
whatever the tool being managed. If you
have not, nothing is assumed: chapter 2
lays out the parts, chapter 3 installs
mise itself, and chapter 4 has it fetch
your first tool.

A first taste, after installing
(chapters 3–4):

```bash
mise use --global node@22
mise exec -- node --version
```

The first command downloads Node (the
newest 22.x) and records it as your
default. The second asks mise to run it,
and it answers with its version:

```text
v22.23.4
```

(from the lab's transcript
`mise-lab/transcripts/t01-first-tool.txt`).
From there, chapter 5 pins a *different*
Node in a project folder and watches the
same `node` command change its answer as
you move between folders.

## Who it is for

The developer juggling projects that each
want a different runtime. The team that
wants "what versions do we use?" answered
by a file in the repo, not by whoever set
up their laptop last. The learner who
wants to try a language for a weekend
without evicting the version their other
work depends on. The common thread is one
person with several projects and one
machine, tired of version soup.

## What it is not

Mise is not a container: it does not
isolate a project from your machine, it
just puts the right binaries in charge
(chapter 13's neighbours cover the
isolating kind). It is not a package
manager for your operating system — it
manages *developer tools*, per user, and
leaves system software alone. It is not a
build system: its tasks (chapter 8) are a
convenience for running a project's own
commands, not a replacement for a real
build tool on a large project. And it is
not magic about versions it cannot get:
some tools it must compile from source,
slowly, and chapter 14 is honest about
which and why.
