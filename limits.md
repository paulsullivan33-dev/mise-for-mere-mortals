# Limits

What mise cannot do, and what it must not
be asked to do — the chapter the rest of
the guide has been promising.

## Some tools arrive as source code

Mise's happiest path is a prebuilt
archive: download, unpack, run — every
install in this guide's lab took under a
minute for that reason. But some core
tools are **compiled from source by
default**, Python being the famous case:
the recipe builds the interpreter on your
machine, which takes minutes, needs a
working compiler chain, and fails in ways
that belong to the build, not to mise.
(Prebuilt variants exist for some of
these; the documentation describes the
settings, and this guide's lab — which
chose Node precisely because it ships
prebuilt — did not exercise them.) On a
small or locked-down machine, a compiling
tool is a real cost to budget for, not a
surprise to discover at 4:55 PM.

## Versions are not isolation

A pin decides *which* binary runs. It
does not decide what that binary can see:
your real home folder, your real network,
your real other projects. Two projects on
the same Node version with conflicting
global packages still share a machine —
mise manages tool versions, not
sandboxes. If a project needs a wall
around it, that is the container tools'
job (chapter 13), and no `.toml` file
changes it.

## The same version can still differ

Mise pins *upstream's* version numbers.
It does not make two builds of "Node
20.20.2" identical if they were built for
different systems, and it does not pin a
tool's *own* dependencies: the system
libraries a binary links against are your
operating system's affair. "Works on my
machine" shrinks under mise — the
toolchain stops being the variable — but
it does not reach zero. Chapter 13 names
the tool whose entire purpose is closing
that last gap, at its price.

## You are trusting a supply chain

Every install is a download of someone
else's binary, executed by you. Mise
reduces the risk in real ways — recipes
point at the projects' own release
channels, the aqua registry's entries are
reviewed in the open, checksums are
verified where upstream publishes them —
but the risk is reduced, not removed. An
asdf-backend plugin is third-party shell
code that runs with your permissions.
That is chapter 1's rule from the other
side: pin only what you would install by
hand, and treat a new backend address in
a project's config with the same care as
a new dependency — because it is one.

## A config file is code near your shell

Tasks run commands. `[env]` shapes the
environment of everything launched in the
folder. Hooks (documented, not run in
this guide) can run on directory changes.
Mise's trust step (chapter 5) is the
mitigation, and it is a good one — but it
moves the responsibility rather than
removing it: `mise trust` on a file you
have not read is the same decision as
running any other unread script. On a
team, review changes to `mise.toml` in
pull requests with the same attention you
give a build script, because that is what
it can be.

## Coverage and platforms

Not every tool is in every backend, and
not every version of every tool was ever
published for your platform — a recipe
can only fetch what upstream built.
Old versions vanish from some release
pages; a pin from five years ago may be
unfetchable today, and mise cannot
conjure it. Windows, finally, is the
younger platform here: the core ideas
work, but some mechanisms (shims
especially) are more recent additions
there, and the fine print in the
documentation is worth reading before a
Windows team standardises on details
this guide — lab-bound to Linux — taught
from the docs rather than from runs.
