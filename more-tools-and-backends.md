# More Tools and Backends

Node was one tool from one backend. This
chapter widens both: a second version of
the same tool living alongside the first,
and a different species of tool entirely
— a small command-line utility fetched by
a different backend — to show that the
commands you already know cover both.

## Another version of the same tool

Nothing about chapter 4's install was a
one-per-customer affair:

```bash
mise install node@20
```

```text
mise ✓ node@20.20.2  29.1s  node-v20.20.2-linux-x64.tar.gz
```

`install` without `use` puts the version
on the shelf without changing any pin —
the preparation step, done before
chapter 5's projects asked for it. The
shelf now holds two Nodes (transcript
`mise-lab/transcripts/t01-first-tool.txt`):

```text
$ mise ls
node  20.20.2
node  22.23.4  ~/.config/mise/config.toml  22
```

Versions are cheap to keep: each is a
folder under `~/.local/share/mise/installs`,
nothing is patched or relocated, and
removing one (chapter 11) leaves the
others untouched. This is the property
the whole guide leans on — the cost of
"try it on the newer Node" becomes a
pin change, not a machine change.

## A tool from the aqua registry

Most tools a developer wants are not
languages. They are small, sharp
utilities — and there are thousands, far
more than any one project team could
write recipes for. Mise's answer is the
**aqua backend**: a community registry
(the aqua project's, baked into the mise
binary — over two thousand tools in this
build) where each tool's entry says where
its official releases live and how to
unpack them. You name the tool by its
registry address:

```bash
mise use --global aqua:BurntSushi/ripgrep@14
```

```text
mise ✓ aqua:BurntSushi/ripgrep@14.1.1  9.1s  ripgrep-14.1.1-x86_64-unknown-linux-musl.tar.gz
```

(transcript
`mise-lab/transcripts/t02-second-backend.txt`).
Ripgrep — a fast text-search tool —
arrived exactly as Node did: resolved
("14" → 14.1.1), downloaded from its own
project's release page, unpacked onto the
shelf, pinned in the global config. From
here it is a citizen like any other:
`mise ls` lists it, `mise which rg` finds
its binary inside its own install folder,
`mise exec -- rg --version` runs it, and
chapter 10's mechanisms will run it by
its plain name, `rg`.

The address is worth a second look.
`aqua:` names the backend, and
`BurntSushi/ripgrep` is the tool's home
repository — the same address you would
visit to download it by hand, which is
chapter 1's rule made visible. Many
popular tools also have **short names**:
`mise registry` lists the ones mise knows
(`node`, `ripgrep` among them), and a
short name quietly chooses a recommended
backend for you. Short names are the
convenient spelling; the full address is
the honest one, and it is what `mise ls`
prints, so you can always see which you
got.

## What the other backends are for

The lab exercised two backends; the
doctor's list (transcript t00) shows the
rest, described here from the
documentation and labelled accordingly:

- **`github:`** fetches a tool straight
  from a named GitHub repository's
  releases — for tools the aqua registry
  has not catalogued, or when you want
  one specific project's builds.
- **`asdf:`** runs the plugin recipes of
  the older asdf manager: a long tail of
  community-maintained tools. Each plugin
  is third-party code that will run on
  your machine — chapter 1's rule applies
  with both hands.
- **Language package backends** —
  `cargo:`, `npm:`, `pypi:`, `gem:`,
  `go:` — install a tool through that
  ecosystem's own publishing system, for
  tools distributed that way.
- **`ubi:`** is the generalist: point it
  at a releases page and it works out the
  right lone binary for your platform.

Which backend a pin uses is part of the
pin: it is written in the config file
(`"aqua:BurntSushi/ripgrep" = "14"` sits
in the lab's global config, transcript
t01). Two projects can even pin the same
tool through different backends and get
identically-behaved, separately-sourced
copies — unusual, but the file makes it
explicit rather than accidental.
