# Installing Mise

Mise itself is a single binary with no
runtime dependencies, and it is installed
the ways single binaries are: an
installer script from the project, an
operating system package, or a release
archive unpacked by hand. This guide's
lab used the third route — the official
release archive from the project's GitHub
releases page, unpacked into a scratch
folder — because it is the route whose
every step can be shown. The installer
script (`curl https://mise.run | sh`) is
the route the project recommends for most
people and ends at the same `mise`
command; like any pipe-to-shell
installer, it deserves a read before it
gets a run (chapter 1's rule applies to
mise itself). Packages exist for the
usual managers — apt, Homebrew, winget
and friends — and the project's
documentation lists them all; they are
described here, not run, and
`criteria.md` keeps that ledger honest.

## The release archive, step by step

On a Linux x86-64 machine:

```bash
mkdir -p ~/mise-bin
curl -LO https://github.com/jdx/mise/releases/download/v2026.10.21/mise-v2026.10.21-linux-x64.tar.xz
tar -xf mise-v2026.10.21-linux-x64.tar.xz -C ~/mise-bin --strip-components=1
~/mise-bin/bin/mise --version
```

Archives for other systems and
architectures sit on the same releases
page (macOS and Windows builds included);
pick the one naming your platform. Put
the `bin` folder on your `PATH` — or move
the `mise` binary somewhere already on
it — and plain `mise` works everywhere.
(The lab left its binary in the scratch
folder and called it by full path when
setting up; everything after that is the
same `mise` you will have.)

Check: the version printout (transcript
`mise-lab/transcripts/t00-version.txt`)
reads:

```text
$ mise --version
2026.10.21 linux-x64 (2026-10-27)
```

Mise versions itself by date, and it is
actively developed — commands gain flags
and defaults shift. This guide's
transcripts are all from **2026.10.21**;
advice written for a much older mise may
name things this one has renamed.

## The doctor, and the folders it keeps

Run `mise doctor` once after installing.
It reports the version, whether your
shell setup is complete, and any problems
it finds — the lab's run ends with "No
problems found". It also names the
folders mise keeps, worth knowing before
you go looking:

| Folder | What lives there |
|---|---|
| `~/.config/mise` | Config: the global `config.toml`, and settings |
| `~/.local/share/mise` | Data: the `installs/` shelf of downloaded tools, and the `shims/` folder |
| `~/.cache/mise` | Download cache — archives kept so reinstalling is cheap |
| `~/.local/state/mise` | State, including which project configs you have trusted (chapter 5) |

Nothing outside your home directory is
touched: mise is a per-user tool. Delete
those folders and mise is gone, tools and
all — there is no uninstaller to hunt
for, and no service to stop, because
nothing runs in the background.

## Keeping mise itself current

A mise installed from the release archive
or the installer script can update itself
with `mise self-update` when a newer
release exists (the doctor mentions it
when one does). A mise installed from an
operating system package updates with
that package manager instead — running
`self-update` there will tell you so.
Either way the update is of the manager
only; the tools it manages are chapter
11's subject, not this one's.
