# Recipes

Small jobs, whole. Each assumes mise
installed (chapter 3) and tools you would
install by hand (chapter 1).

## Try a language for the weekend

Curiosity without commitment — install
nothing globally, change no pins:

```bash
mise exec node@22 -- node --version
mise exec aqua:BurntSushi/ripgrep@14 -- rg --version
```

Each command downloads the tool if needed
and runs it once. If the weekend becomes
a project, `mise use` writes the pin;
until then your machine's defaults never
moved.

## Onboard a fresh clone

The whole setup of a mise-managed
project, in the order a new contributor
meets it:

```bash
git clone <the project>
cd <the project>
cat mise.toml        # read first — chapter 5's rule
mise trust           # approve what you just read
mise install         # fetch every pinned tool
mise tasks           # see how the project runs itself
```

Every later session is free: the pins,
the environment and the tasks are already
in force whenever work happens in the
folder.

## Match CI to your laptop

CI failures that "work on my machine" are
often version drift wearing a trench
coat. If the project's `mise.toml` is the
single source of truth, CI can install
the same mise, run `mise install`, and
then run the project's own tasks — the
identical toolchain locally and remotely.
(Mise's documentation describes generating
ready-made CI and devcontainer
configuration from a project's config;
this guide's lab did not generate one,
and the claim here is the principle, not
the generator.)

## One tool, two projects, no drama

The situation from chapter 5, as a
standing habit rather than a fix: when a
project breaks after a system upgrade,
do not downgrade the machine — pin the
project. `mise use node@20` inside it,
and the rest of your work keeps the newer
default. The shelf holds both; the files
keep them straight.

## Find out what a command really is

A `node` on your machine might be the
system's, a shim's choice, or a leftover
from another manager:

```bash
which node
mise which node
```

The first shows what your shell found;
the second shows what mise *would* run
here. When they disagree, chapter 6's
rules explain why, and chapter 15 walks
the cases.

## A scratch version, once

Testing whether a bug is version-shaped,
without touching any pin:

```bash
mise exec node@20.19.0 -- node app.js
```

works through the project's own code with
one exact Node, then leaves everything as
it was. If the bug moves with the
version, you have learned something no
amount of staring at the code would have
told you.
