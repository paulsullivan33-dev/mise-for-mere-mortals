# Tasks

Every project accumulates incantations:
the command that runs the tests, the one
that builds the report, the three-step
one only one person remembers. Mise's
config file can hold them as **tasks** —
named commands, defined by the project,
run with `mise run <name>` by anyone,
with the project's pinned tools already
in charge. The outcome of this chapter:
the lab's fixture project lists its
tasks, runs one with a dependency, and
runs another that lives in a file of its
own.

## Tasks in the config file

The lab's `report-generator` fixture
defines two (transcript
`mise-lab/transcripts/t03-project-pins.txt`
shows the file whole):

```toml
[tasks.check]
description = "Show the Node version this project is pinned to"
run = "node --version"

[tasks.report]
description = "Generate the summary report"
depends = ["check"]
run = "node report.js"
```

A task is a name, an optional
`description` (this is what listings
show), and the command to run. `depends`
names tasks that must complete first —
the project's own ordering, stated once
in the file instead of re-typed at every
call. List what a project offers with
`mise tasks`:

```text
$ mise tasks
check   Show the Node version this project is pinned to
report  Generate the summary report
tidy
```

and run one:

```text
$ mise run report
[check] $ node --version
v22.23.4
[report] $ node report.js
Report: 3 product lines counted.
Generated with Node v22.23.4.
```

Worth noticing: the `check` task's whole
job is printing which Node the project is
pinned to, and it answered 22.23.4 —
tasks run **inside the project's
toolset**, with the pins of chapter 5 in
force and the project's environment
(chapter 9) set. A task therefore behaves
the same for every runner, which is the
entire point of writing it down. The
`[name] $ command` lines mise prints
before each step go to the error stream,
so a task's real output stays clean for
pipes and files.

## File tasks

A task that outgrows a one-line `run`
belongs in a file. Any **executable**
script in a project's `mise-tasks/`
folder becomes a task named after the
file. The fixture's is deliberately
small:

```bash
#!/usr/bin/env bash
# A file task: anything executable in mise-tasks/ becomes a task.
echo "Tidying generated files..."
```

It appears in `mise tasks` as `tidy` and
runs the same way:

```text
$ mise run tidy
Tidying generated files...
```

Any language with a `#!` line works —
shell, Python, Node itself. Two honest
warnings from the lab's notes. The file
**must be executable** (`chmod +x`): a
file task that is not is silently absent
from the listing, no error, just missing
— chapter 15 has it as an entry, because
the lab lost ten minutes to it. And a
file task is a script like any other:
read the ones in a cloned project before
running them, which is chapter 5's trust
step doing double duty.

## Where tasks fit, and where they don't

Tasks are at their best as the project's
*front door*: `setup`, `test`, `build`,
`report` — the handful of commands every
contributor needs, spelled once. They are
not a build system. A project whose build
has hundreds of steps, generated files
and fine-grained dependencies wants a
real build tool, and mise's honest role
there is the one from chapters 4–7:
making sure the right version of that
tool — and of everything it calls — is
what runs.
