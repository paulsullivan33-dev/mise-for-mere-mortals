# Keeping Tools Current

Pins freeze a project in time on purpose;
this chapter is the deliberate thaw. The
outcome: you can see which of your tools
are behind, how far behind, and move a
pin — within its range, or beyond it —
as a decision rather than an accident.

## Seeing what exists

`mise ls-remote` asks a backend what
versions of a tool there *are*:

```text
$ mise ls-remote node | tail -4
26.11.0
26.11.1
26.12.0
26.12.1
```

(the full list is long; the tail is the
frontier). The lab's shelf is Node-22-era,
and the remote list calmly reports that
Node has moved on to 26 — a useful
reminder that a pin says what a project
*uses*, not what the world has settled
on.

## Behind, inside your ranges

`mise outdated` compares each pin with
the newest version **the pin itself
allows**. The lab ran it in
`pinned-project`, whose Node is fixed to
the exact 20.19.0 (transcript
`mise-lab/transcripts/t09-outdated.txt`):

```text
$ mise outdated
mise All tools are up to date
mise Newer versions are available outside the configured version ranges. Use `mise outdated --bump` to view them.
```

Both sentences are true at once, and the
distinction is the feature. 20.19.0 is
the newest 20.19.0 there will ever be —
within its exact range, nothing to do.
Outside the range, the world has moved.
A looser pin behaves differently: a
project pinning `node = "20"` would be
told about 20.20.2 the day it appeared,
because moving within the 20 line is what
that pin *means*.

## How far a bump would go

`mise outdated --bump` shows the other
view — current against newest overall:

```text
$ mise outdated --bump
node                     20.19.0  20.19.0  26.12.1  26.12.1 ~/workspace/mise-for-mere-mortals/mise-lab/projects/pinned-project/mise.toml
aqua:BurntSushi/ripgrep  14       14.1.1   15       15.2.0  ~/.config/mise/config.toml
```

Read the columns as: tool, what was
*requested*, what is *installed*, and —
twice, for the range view and the bump
view — the newest that exists. The table
is the upgrade decision on one screen:
bumping `pinned-project` is not a patch,
it is Node 20 → 26, three major lines at
a jump, and the table makes sure you know
that before anything moves.

## Moving, and tidying

- **`mise upgrade`** moves pins to the
  newest version *within their ranges*
  and installs them — the routine
  freshening. Adding `--bump` rewrites
  the pins themselves to the newest
  overall; run it in a project whose
  pin you mean to change, and let the
  diff in `mise.toml` be the record of
  the decision.
- **`mise uninstall node@18.20.8`**
  removes one installed version (the
  lab used it to set up chapter 10's
  auto-install demonstration). Pins that
  name it will simply auto-install it
  again on next use, or error if you
  turned auto-install off.
- **`mise prune`** removes installed
  versions that no config anywhere asks
  for — the shelf-tidy for a machine
  that has been through many projects.

The habit this chapter recommends is
small: `mise outdated` when you sit down
in a project you have not touched in a
while, `mise upgrade` when the answer is
boring, and the `--bump` table open in
front of you — read, not skimmed — on
the day the answer is not.
