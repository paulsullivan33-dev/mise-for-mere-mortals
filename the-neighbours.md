# The Neighbours

Mise is not the first tool to notice that
developers drown in versions, and several
of its neighbours solve a piece of the
problem better than it does. An honest
map:

## The single-language managers

**nvm** (Node), **pyenv** (Python),
**rbenv** (Ruby), **rustup** (Rust) and
their kin each manage one language,
deeply, and predate mise by years. Their
depth is real: pyenv's feel for Python
builds, rustup's toolchain channels, are
the work of communities focused on one
ecosystem. If you live entirely inside
one language, its manager may fit like a
glove. Mise's counter-offer is breadth
with one syntax: the same `mise.toml`
that pins your Node pins your Python,
your Go, and the utilities besides — and
chapter 5 showed it reading the older
`.tool-versions` files these managers'
world produced.

## asdf

The direct ancestor: **asdf** generalised
version management first, with a plugin
for everything and the `.tool-versions`
format mise still honours. Mise began as
a faster, single-binary rethinking of the
same idea (it was first released as
"rtx"), adding built-in backends so
common tools need no plugin, plus the
tasks and environment features of
chapters 8–9. asdf's plugin catalogue
remains enormous, and mise can run those
plugins through its asdf backend — the
two are relatives, not enemies.

## Volta and proto

**Volta** manages Node (and npm/yarn)
with a shim-first design and pins per
project, elegantly, inside the JavaScript
world only. **proto** is the closest
cousin: one manager, many languages,
project pins, from the Moonrepo project —
the honest difference is in registries,
task features and taste. Either is a
reasonable choice; this guide teaches
mise, and chapter 5's legacy-file support
means a team can drift between managers
without a flag day.

## Devcontainers and Docker

A different answer to the same pain: put
the toolchain in a **container**, and the
version problem dissolves into an image
reference. Containers isolate — files,
network, system libraries — which mise
pointedly does not (chapter 14). The cost
is weight: an image per project, editors
and tools reaching across the container
wall, and a slower loop for small work.
Many teams land on both: containers for
the deployable truth, mise for the
laptop's daily comfort. They compose
fine — mise runs happily inside a
container, where its `mise install` reads
the same `mise.toml`.

## Nix

The principled extreme: **Nix** describes
entire environments — compilers,
libraries, system tools — as reproducible
expressions, bit-for-bit, on any machine.
Nothing else on this page goes as far,
and nothing else asks as much: a new
language, a new mental model, a new
everything. Mise is the shallower tool by
design: it manages versions of tools, not
the universe they link against. If your
pain is "the same version behaves
differently on two machines", that is Nix
country and chapter 14 says so plainly.

## direnv

Not a version manager at all: **direnv**
loads and unloads environment variables
as you move between folders, from a
per-project `.envrc`. Mise's `[env]`
(chapter 9) covers the common cases in
the same file as the pins, and even its
trust rule is direnv's shape. A project
with serious environment machinery —
secrets fetched at shell time, elaborate
conditionals — may still want direnv
proper, and the two coexist: a version
from mise, an environment from direnv,
each doing the half it is best at.
