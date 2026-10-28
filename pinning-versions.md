# Pinning Versions Per Project

The outcome of this chapter: two projects
side by side, one pinned to Node 20 and
one to Node 22, and mise reporting each
project's choice correctly. The actual
switching — `node` changing its answer as
you move — is chapter 10; this chapter is
about the pins themselves, because the
pin is the artifact your team keeps.

## Writing a pin

Inside a project folder, the same `use`
command without `--global` writes a
project config instead:

```bash
cd ~/projects/inventory-api
mise use node@20
```

```text
mise ~/projects/inventory-api/mise.toml tools: node@20.20.2
```

and the folder now holds a `mise.toml`:

```toml
[tools]
node = "20"
```

The lab's fixtures are in
`mise-lab/projects/` and their files are
shown whole in transcript
`mise-lab/transcripts/t03-project-pins.txt`:
`inventory-api` pins Node 20 (an older
project, staying on the line it was built
with), `report-generator` pins Node 22.
Asking for "20" installed 20.20.2, the
newest of that line, and the file records
the *request* — "20" — not the resolved
patch version. That choice is deliberate:
the pin says *the 20 line*, and chapter
11 shows how to move within or beyond it.
When a project truly needs one exact
patch release, the file can say so — the
lab's `pinned-project` fixture pins
`node = "20.19.0"`, and chapter 11 uses it
to show what "out of date" means at that
strictness.

Commit the `mise.toml`. It is small,
readable in a diff, and it is the answer
to "what does this project need?" for
everyone who clones the repo — including
a CI server, which can run the same
`mise install` and get the same tools.

## The trust step

There is one surprise waiting in a
freshly cloned project, and the lab met
it head-on (transcript
`mise-lab/transcripts/t11-trust.txt`).
A `mise.toml` can carry more than pins —
tasks that run commands, environment
entries, hooks (chapters 8 and 9). So a
config that mise did not write itself,
and has never been shown, is only partly
honoured: the `[tools]` pins are read,
but the parts that can *act* are refused
until you approve the file:

```text
$ mise current
mise ERROR error parsing config file: ~/projects/inventory-api/mise.toml
mise ERROR Config files in ~/projects/inventory-api/mise.toml are not trusted.
Trust them with `mise trust`. See https://mise.jdx.dev/cli/trust.html for more information.
```

The fix is one command, run in the
project after you have **read the file**
— that reading is the point of the
exercise, not a hoop before it:

```bash
mise trust
```

```text
mise trusted ~/projects/inventory-api
```

and everything in the file takes effect.
This is chapter 1's rule wearing overalls:
a config file from a stranger's repo is
code you are about to run, and mise makes
the decision explicit instead of silent.
Your own configs, written by `mise use`,
are trusted as they are written; the
prompt appears for files that arrive by
clone, copy or edit — and reappears if a
trusted file's contents change under you,
which is the behaviour you want.

## The legacy file

Projects that predate mise often carry an
**`.tool-versions`** file — the format of
the older asdf manager (chapter 13), one
`tool version` pair per line. Mise reads
those too, with no conversion and no
ceremony. The lab's `legacy-project`
fixture holds a single line —

```text
nodejs 20
```

— and mise honours it exactly like a pin
(transcript
`mise-lab/transcripts/t08-legacy-file.txt`):
`mise current node` reports 20.20.2 (note
the older name, `nodejs`, is understood)
and the tools run at that version. Where
both files exist, `mise.toml` wins for
anything it names. A team can therefore
migrate one project at a time, or not at
all: the file they already have keeps
working while new projects start on
`mise.toml`.
