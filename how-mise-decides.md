# How Mise Decides

Several config files can speak at once:
a global default, a project's pin, a pin
in a folder above the project. This
chapter is the rulebook mise follows to
produce one answer — and the three
commands that show its working, so a
surprising version is never a mystery.

## The rules

1. **Nearest wins.** Mise looks for
   config files starting in the folder
   you are standing in and walking up
   towards the root. The first file that
   names a tool settles that tool's
   version. A project pin therefore beats
   the global config, and a pin in a
   subfolder would beat its own project's.
2. **Files combine per tool.** The walk
   does not stop at the first file: it
   collects them all, and each tool's
   version comes from the nearest file
   that names *that tool*. Your global
   ripgrep stays in force inside a
   project that only pins Node — the lab's
   `mise current` output below shows
   exactly that, with the global tool
   listed alongside the project's Node.
3. **Subfolders inherit.** Nothing about
   the folder you stand in needs to be
   the project's root. The lab stood two
   levels below `inventory-api`'s root —
   in `src/deep`, beside no config at
   all — and the pin still applied,
   because the walk passes through it.
4. **The environment can override.**
   Variables of the form `MISE_NODE_VERSION`
   (and a handful of settings) override
   files for that shell. Useful for a
   one-off test; a fine way to confuse
   yourself if left exported. Chapter 15
   checks for them first when a version
   looks haunted.

## Seeing the working

Three commands, all run by the lab in the
fixture projects (transcript
`mise-lab/transcripts/t03-project-pins.txt`).

**`mise current`** — the resolved
versions that apply here:

```text
$ cd inventory-api && mise current
node 20.20.2
aqua:BurntSushi/ripgrep 14.1.1
```

Both tools that apply are listed: the
project's Node 20 (resolved to 20.20.2)
and the global ripgrep, inherited because
the project does not name it. Ask about
one tool — `mise current node` — and you
get just its version, `20.20.2`, which is
the form scripts want.

**`mise config`** — the files in play,
and which tools each speaks for:

```text
$ mise config
~/.config/mise/config.toml  aqua:BurntSushi/ripgrep, node
~/workspace/mise-for-mere-mortals/mise-lab/projects/inventory-api/mise.toml  node
```

When a version surprises you, this is the
first command to run: the file you forgot
— a pin in a parent folder, a global you
set last spring — is standing right there
in the list.

**`mise which`** — the end of the chain,
the actual binary that will run:

```text
$ mise which node
/home/hatch/.local/share/mise/installs/node/20/bin/node
```

(Ask for a tool by the name you run —
`mise which rg` finds ripgrep's binary
just as directly, inside its aqua install
folder; transcript t02.) If a question
about versions ever gets philosophical,
`which` settles it in the physical world:
this file, in this folder, is what
executes.

Check: standing in `inventory-api/src/deep`,
the lab got `mise current node` →
`20.20.2` and `node --version` →
`v20.20.2` — rules 1 to 3 in a single
glance. The same commands at home report
the global Node 22, and in
`report-generator` the project's 22. One
machine, three answers, each traceable
to a file you can open.
