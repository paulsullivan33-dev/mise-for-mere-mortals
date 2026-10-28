# Writing criteria for this guide

Every "for Mere Mortals" guide is written to the same nine-point
standard, so a reader who trusts one can trust the rest. Here it
is, with the verification notes for this particular guide below.

1. **Fix it first.** Reader outcomes lead, not action items: each
   chapter opens by naming the outcome it delivers, shows a check
   the reader can run to confirm they got it, and a reasonable
   reader finishes able to state what was done, what it means,
   and what to try next.
2. **Plain language.** Short sentences, everyday vocabulary,
   technical terms defined on first use. No slang, and no
   idioms that only make sense in one country — the reader may
   be translating as they go.
3. **No dead ends.** A guide never tells the reader to do
   something it cannot show them how to verify. Every claim a
   reader relies on — an install, a command, an output — is
   something this guide demonstrates or clearly labels
   documented-only below.
4. **Live-verified where it matters.** Procedures are run for
   real in the guide's lab wherever the build machine allows it,
   and the transcripts kept in the repository are the evidence.
   What could not be run is listed honestly below, never
   presented as if it had been.
5. **General audiences.** Nothing in this guide — examples,
   fixtures, names, addresses — is modelled on any particular
   reader or author's real life, network, or organisations.
   The lab's traffic is the lab talking to itself.
6. **Standalone.** A reader needs this guide, its lab, and
   nothing else. Where another guide would genuinely help, it
   is pointed to once, by name, and the chapter still completes
   without it.
7. **Honest about limits.** Chapter 14 names what the tool
   cannot do: tools that arrive as source code to compile,
   versions that are not isolation, the same version number
   covering different builds, the supply chain every pin
   trusts, a config file that is code near your shell, and
   the platforms and old versions no recipe can conjure back.
8. **The ethics rule is in the text, early.** Chapter 1 and the
   README state the rule plainly — *pin only tools you would
   install by hand, and read a project's config before you
   trust it* — before any procedure depends on it, and later
   chapters point back to it where the tooling would make
   crossing the line easy.
9. **Builds clean.** The site builds with MkDocs in strict mode
   with no warnings, and a fresh reader following the chapters
   in order reaches the stated outcome of each.

## Verification notes for this guide

The build machine is a small Ubuntu sandbox (Linux x86-64).
mise **2026.10.21** was run from the project's official release
archive for Linux x64, downloaded from its GitHub releases into
a scratch folder — no system-wide install. The tools mise
itself installed went to the usual per-user places
(`~/.local/share/mise`). All transcripts live in
`mise-lab/transcripts/`; the fixture projects are
`mise-lab/projects/`.

Run for real, with transcripts as evidence:

- Version and `mise doctor`: the version, the directories mise
  keeps, the backends baked into this build, and "No problems
  found" (t00).
- First tool: `mise use --global node@22` resolving to Node
  22.23.4 from the core backend's prebuilt archive, the global
  config it wrote, `mise ls`, and Node and npm answering
  through `mise exec` (t01).
- A second version and a second backend: Node 20.20.2
  installed alongside, and ripgrep 14.1.1 through the aqua
  backend from its project's own release archive, located
  with `mise which` and run with `mise exec` (t02).
- Project pins: the fixture `mise.toml` files, `mise current`
  inside a project (project Node and inherited global tool
  listed together), the pin still applying two folders below
  the project root, and `mise config` listing the files in
  play (t03).
- Switching by shims: with the shims folder on `PATH`,
  `node --version` answering 22.23.4 at home, 20.20.2 in
  inventory-api, 22.23.4 in report-generator, 22.23.4 back
  home (t04).
- Switching by activation: the same walk in an interactive
  bash session with `mise activate` in force, recorded with
  the `script` utility (t05).
- Tasks: `mise tasks` listing two config tasks and one file
  task, `mise run report` running its `depends` task first
  and then the report under the pinned Node, and `mise run
  tidy` running the file task (t06).
- Environment: the fixture's `[env]` entry shown by
  `mise env` as an export line and found by `printenv`
  inside `mise exec` (t07).
- The legacy file: a `.tool-versions` reading `nodejs 20`
  honoured as a pin, resolving to 20.20.2 (t08).
- Keeping current: Node 20.19.0 installed for an exactly
  pinned fixture; `mise outdated` reporting all up to date
  inside its range, `mise outdated --bump` showing the jump
  beyond it (Node to 26.12.1, ripgrep to 15.2.0), and
  `mise ls-remote node` (t09).
- Auto-install: Node 18 uninstalled, then a shim call inside
  the project pinning 18 downloading and installing 18.20.8
  before answering `v18.20.8` (t10).
- Trust: a fixture config carrying `[env]`, written by an
  editor rather than by mise, refused as not trusted —
  pins read, acting parts withheld — until `mise trust`
  approved it (t11).

Documented only — described from the project's documentation,
not run, and the text does not claim otherwise:

- The other install routes: the `https://mise.run` installer
  script, operating-system packages, and `mise self-update`.
- Activation lines for zsh, fish and PowerShell, and Windows
  behaviour generally — the lab is a Linux/bash machine, and
  shims on Windows are described as the newer addition the
  documentation says they are.
- Python and the other tools mise compiles from source by
  default, and the prebuilt variants and settings for them:
  the lab deliberately chose Node because its core recipe
  downloads prebuilt archives.
- The backends beyond core and aqua as exercised: `github`,
  the asdf plugin backend and its third-party plugins,
  `cargo`, `npm`, `pypi`, `gem`, `go` and `ubi` — named,
  characterised, not run.
- Environment values built from templates, env files loaded
  by reference, and config hooks.
- Generating devcontainer and CI configuration from a
  project's config; the recipes chapter teaches the
  principle (the same `mise.toml` everywhere), not the
  generator.
- The comparison tools in chapter 13 are characterised, not
  benchmarked.

Fixtures are this guide's own and deliberately generic: five
small invented projects (an inventory API pinned to the Node
line it was built with, a report generator on the current
line, a legacy project carrying an asdf file, an exactly
pinned project, and one pinning an older Node the lab removed
and let mise fetch back). No example is modelled on any real
person, network, service or organisation, and every tool the
lab installed came from that tool's own official release
channel.
