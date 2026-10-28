# Shims, Activation, and Exec

Everything so far has been preparation:
pins written, tools installed, decisions
explained. This chapter is the payoff —
the three ways `node` typed at a prompt
becomes the Node the config asked for —
each demonstrated by the lab walking
between the fixture projects and watching
the version change.

## Way one: shims

A **shim** is a tiny stand-in program
with a tool's name. Mise keeps a folder
of them — `~/.local/share/mise/shims`,
one per tool it manages — and you put
that folder on your `PATH` once:

```bash
export PATH="$HOME/.local/share/mise/shims:$PATH"
```

From then on, typing `node` runs the
shim; the shim asks mise what applies in
the current folder, and hands over to the
real binary. The lab's walk (transcript
`mise-lab/transcripts/t04-switching-shims.txt`)
is the whole feature in four lines:

```text
$ cd ~ && node --version
v22.23.4
$ cd mise-lab/projects/inventory-api && node --version
v20.20.2
$ cd ../report-generator && node --version
v22.23.4
$ cd ~ && node --version
v22.23.4
```

No command runs between the `cd`s. The
version follows the folder because the
question "what applies here?" is asked
again at every invocation, with chapter
6's rules. Shims ask nothing of your
shell beyond the one `PATH` line, work
in scripts and editors that spawn their
own shells, and cost a few milliseconds
per call. Their one blind spot: a program
that was *already running* keeps the
tools it started with — the shim decides
at launch, not continuously.

## Way two: activation

**Activation** trades the shim's
per-call question for a resident
arrangement with your shell. One line in
your shell's startup file —

```bash
eval "$(mise activate bash)"
```

(zsh and fish have their own spellings in
the documentation) — and mise hooks into
the shell itself: as you change folders
it rewrites the environment — `PATH`
pointing at the applicable installs, the
`[env]` variables of chapter 9 set — so
the plain `node` on your `PATH` *is* the
real binary, no shim in between. The lab
ran an interactive session this way and
walked the same route (transcript
`mise-lab/transcripts/t05-activation.txt`);
the answers were identical: 22.23.4,
20.20.2, 22.23.4, 22.23.4.

Choose activation if you live in one
shell and want the environment itself —
variables included — to follow you.
Choose shims if you want one `PATH` line
to cover every shell and script on the
machine. The two can coexist; the lab's
machine had shims on `PATH` throughout,
and activation layered cleanly on top.

## Way three: exec

**`mise exec`** needs no setup at all and
chapter 4 already used it:

```bash
mise exec -- node --version
```

It is the right tool for scripts, CI jobs
and one-off questions — anywhere "set up
my shell" is the wrong answer to "run
this once". It also takes an explicit
tool for a single run, ignoring the pins:

```text
$ mise exec node@20 -- node --version
v20.20.2
```

run from a folder whose pin said 22. The
override lasts exactly one command; the
config file is untouched. (`mise run`,
chapter 8, is exec's sibling: the same
setup, aimed at the project's own tasks.)

## The missing version installs itself

One more behaviour belongs in this
chapter, because all three ways share
it. Walk into a project whose pin has
never been installed — the lab's
`future-project`, pinning Node 18 with
Node 18 deliberately removed — and simply
ask for the tool (transcript
`mise-lab/transcripts/t10-auto-install.txt`):

```text
$ cd mise-lab/projects/future-project && node --version
mise ✓ node@18.20.8  node-v18.20.8-linux-x64.tar.gz
v18.20.8
```

Mise noticed the gap, downloaded and
installed 18.20.8, then answered. This is
the **auto-install** setting, on by
default in this build (`mise settings get
auto_install` says `true`), and it is
what makes a fresh clone workable: clone,
`cd`, run — the toolchain assembles
itself on first use. It is also a
download you did not explicitly request,
which cuts both ways: on a metered or
offline machine, or where downloads must
be approved, turn it off (`auto_install =
false` in the global settings) and let
`mise install` be a deliberate step
whose absence produces an honest error
instead of a quiet wait.
