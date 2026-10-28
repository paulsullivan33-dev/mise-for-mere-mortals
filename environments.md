# Environments

Tools are half of what a project needs to
agree on; the other half is the
**environment variables** the tools read —
which database, which mode, which stage.
Mise's config file can carry those too,
in an `[env]` table, so the values travel
with the pins they belong to. The outcome
of this chapter: a variable defined in a
project's config, shown to be present
when mise works in that project and
absent outside it.

## The [env] table

The lab's `inventory-api` fixture carries
one entry (its file is shown whole in
transcript t03):

```toml
[env]
INVENTORY_ENV = "demonstration"
```

Ask mise what environment it would set
up here:

```bash
mise env
```

```text
$ mise env
export INVENTORY_ENV=demonstration
```

(transcript
`mise-lab/transcripts/t07-env.txt`). The
output is shell code — a list of `export`
lines for everything the applicable
configs define. With shell activation
(chapter 10) those exports happen
automatically as you enter the folder;
without it, `mise env` is how you see
what *would* be set, and the form scripts
want is `mise env --json`.

The proof that it reaches real work is
the same shape as chapter 4's:

```text
$ mise exec -- printenv INVENTORY_ENV
demonstration
```

Inside the project's toolset, the
variable is simply *there* — for the
project's tasks (chapter 8) as much as
for commands you run by hand. Outside the
project, or in a project that defines
nothing, the same `printenv` prints
nothing: the variable is not set
machine-wide, not written into your
shell's startup, just lent to the work
while mise is in charge of it.

## How far this goes — and the honest fence

An `[env]` entry can be more than a
literal string: mise's documentation
describes values built from templates,
variables drawn from files, and a
project's `.env`-style file loaded by
reference. Those forms are **documented
only** in this guide — the lab ran
literal values, and the text does not
pretend otherwise. Two cautions apply to
the whole feature, simple or fancy:

- **A value in `mise.toml` is a value in
  the repo.** Real secrets — production
  passwords, API keys — do not belong in
  a committed config file, however
  convenient the symmetry feels. The
  `[env]` table is for the settings a
  project is happy to publish: modes,
  names, local paths, feature flags.
- **Environment is action.** A config
  that can set variables can point a
  tool at a different server, which is
  why [env] sits behind the trust step
  of chapter 5 alongside tasks. Read
  first, `mise trust` second — in that
  order, every time.
