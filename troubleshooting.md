# Troubleshooting

Symptom first, cause second, in the order
the lab and the documentation suggest you
check them.

## The wrong version runs

*You are standing in the project, the pin
says 20, and `node --version` says
something else entirely.*

1. **You are not using mise's answer at
   all.** No shims on `PATH`, no
   activation, and your shell found a
   system Node first. `which node` shows
   whose binary answered; chapter 10 is
   the fix, in any of its three flavours.
2. **Another config speaks louder.**
   `mise config` lists every file in
   play. A pin in a parent folder, or a
   global you set and forgot, may be the
   nearer voice (chapter 6's rules).
3. **An override is exported.**
   `MISE_NODE_VERSION` (and kin) beat the
   files. `env | grep MISE_` shows them;
   unset and the haunting ends.
4. **The program was already running.**
   Shims and activation decide at launch.
   Restart the editor or terminal that
   started before the pin changed.

## "Config files are not trusted"

*Mise refuses parts of a project's
config — the lab's own welcome to the
fixture projects (transcript t11).*

The config carries `[env]`, tasks or
hooks, and mise did not write it and has
never been shown it. Read the file —
really read it, it is about to run
commands on your machine — then
`mise trust` in the project folder.
Pure `[tools]` pins never trigger this;
if a tools-only project shows it, the
file contains more than pins, which is
itself the thing worth knowing.

## The tool is not found after installing

*`mise ls` shows it; the shell says
`command not found`.*

1. **Nothing connects the shell to the
   shelf yet.** Installs alone do not
   change your `PATH`. Add the shims
   folder or activate (chapter 10), or
   run the tool through `mise exec`.
2. **The tool's name is not its command.**
   Ripgrep installs as the tool
   `ripgrep` and runs as `rg`. `mise
   which rg` — ask for the command name —
   finds it; `mise ls` shows the tool
   name.
3. **The shell cached the old world.**
   Long-lived shells remember where
   commands were (bash's `hash`). A new
   terminal, or `hash -r`, clears it.

## A task is missing from the listing

*`mise tasks` does not show a task you
can see in the folder.*

1. **The file task is not executable.**
   File tasks in `mise-tasks/` need
   `chmod +x`; without it they are
   silently absent — no error, just not
   there. The lab learned this one
   personally.
2. **The config is untrusted.** Tasks in
   an untrusted `mise.toml` do not run
   and may not list; see the trust entry
   above.
3. **You are in the wrong folder.** Tasks
   belong to the project whose config
   defines them; `mise tasks` answers
   for *here*.

## An install fails or crawls

*Downloads stall, or a build runs for
many minutes.*

1. **The tool compiles.** Some backends
   build from source (chapter 14):
   minutes are normal, and the failure at
   the end is a missing compiler or
   library, named in the build output —
   a machine problem, not a mise problem.
2. **The version does not exist.**
   `mise ls-remote <tool>` shows what
   does. A typo in a pin ("2O" with a
   letter O has happened to everyone)
   reads exactly like a missing release.
3. **The shelf is confused.** Rare, and
   blunt instruments work: `mise uninstall
   <tool>@<version>`, then `mise install`
   again. Downloads are cached, so the
   second trip is usually quick.

## Two managers, one tool

*An old nvm/pyenv/asdf setup and mise
both claim the same command.*

Shells resolve commands by `PATH` order,
and startup files append in the order
they run. Decide which manager owns the
tool, remove the other's claim (its
activation lines in your shell's startup
are the usual place), and let chapter 5's
legacy support carry the old project's
files. Running both is not wrong, but
running both *unknowingly* is where
haunted versions come from.
