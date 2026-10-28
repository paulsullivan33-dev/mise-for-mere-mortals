# Your First Tool

The outcome of this chapter: one real
tool, Node, downloaded and installed by
mise, set as your machine-wide default,
and answering commands — with every step
checkable as you go. Node is the
demonstration because mise's core backend
downloads its prebuilt archives: nothing
compiles, so the chapter is about mise,
not about waiting.

## Installing and setting a default

One command does both jobs:

```bash
mise use --global node@22
```

Read it left to right. `use` means
*install this if needed, and record it as
the version to use*. `--global` says the
recording goes in the global config
(chapter 2), so it applies everywhere no
project overrides it. `node@22` names the
tool and asks for the newest release in
the 22 line. The lab's run (transcript
`mise-lab/transcripts/t01-first-tool.txt`)
shows what happened:

```text
$ mise use --global node@22
mise node@22.23.4 10.9.9
mise ✓ node@22.23.4  31.5s  node-v22.23.4-linux-x64.tar.gz
mise ~/.config/mise/config.toml tools: node@22.23.4
```

Three facts to take from that. The
request "22" resolved to a full version,
**22.23.4** — mise always installs full
versions; partial numbers are how you
*ask*, not what you get. The archive name
is the one from nodejs.org: the core
backend fetched Node from its own
project's shelf, exactly as you would by
hand. And the last line names the file
that now holds the pin:

```text
$ cat ~/.config/mise/config.toml
[tools]
node = "22"
```

(the ripgrep line in the lab's copy of
this file arrives in chapter 7). That
small file is the whole of your global
setup — readable, editable, and safe to
keep in a dotfiles repo if you keep one.

## Running the tool

You have not touched your shell's
configuration yet, so `node` by itself
may still be an old system copy — or
nothing at all. Chapter 10 covers the
three ways to close that gap for good.
For now, the zero-setup way: ask mise to
run it.

```bash
mise exec -- node --version
```

```text
$ mise exec -- node --version
v22.23.4
$ mise exec -- npm --version
10.9.9
```

The `--` separates mise's own arguments
from the command to run. `mise exec`
works out which versions apply here (the
global config, since you are not in a
project), puts those tools at the front
of the command's `PATH`, and runs it.
Node's companion **npm** came along in
the same archive — tools bring their own
households with them.

Check where things stand at any time
with `mise ls`:

```text
$ mise ls
node  22.23.4  ~/.config/mise/config.toml  22
```

(the lab's fuller table, with a second
Node and ripgrep on the shelf, is in
transcript t01). The columns say what is
installed, which config asks for it, and
what was asked: the shelf can hold many
versions of a tool, and the config
columns tell you which ones are spoken
for. A version with no config beside it
is installed but currently unused —
harmless, and chapter 11 shows how to
tidy them away.

## What just happened, in one paragraph

`mise use` resolved "22" against the list
of releases the backend knows, downloaded
the matching archive from the tool's own
project, verified and unpacked it into
`~/.local/share/mise/installs/node/22.23.4`,
and wrote one line into your global
config. Nothing was compiled, nothing
outside your home folder changed, and the
system's own Node — if you had one — is
still exactly where it was, merely no
longer the default answer. Chapter 5 now
does the interesting part: giving one
project a *different* answer.
