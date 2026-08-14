---
title: Configuration
description: AKM configuration reference and machine layout.
---

## Config File

All AKM configuration lives in a single TOML file:

```
~/.config/akm/config.toml
```

The file uses [TOML](https://toml.io/) format and is created by `akm setup` or
on first run with defaults:

```toml
features = ["skills", "artifacts", "instructions"]

[registry]
url = "git@github.com:you/your-registry.git"

[shared]
acme = "git@github.com:acme/skills.git"

[artifacts]
remote = "git@github.com:you/artifacts.git"
dir = "~/.akm/artifacts"
auto_push = true
```

`[registry]` holds the single writable personal registry — the one that *is*
your library. `[shared]` holds read-only registries to import from, one line per
registry, named however you like. See [Shared Registries](/docs/shared-registries/).

## Config Keys

Keys are hyphenated on the command line and underscored in the TOML file
(`akm config registry.url` reads `registry.url`; `akm config artifacts.auto-push`
reads `artifacts.auto_push`).

| Key | Description | Default |
|-----|-------------|---------|
| `features` | Enabled domains (comma-separated: `skills`, `artifacts`, `instructions`) | `skills` |
| `registry.url` | Git remote for your personal registry (the canonical key) | *(set during setup)* |
| `shared.<name>` | Git remote of a shared registry to import from (empty value removes it) | *(none)* |
| `artifacts.remote` | Git remote for the artifacts repo | *(set during setup)* |
| `artifacts.dir` | Local artifacts directory | `~/.akm/artifacts` |
| `artifacts.auto-push` | Auto commit+push artifacts on session exit | `true` |
| `update.url` | GitHub Releases API URL used by `akm update` | akm-rs latest release |
| `update.check-interval` | Seconds between background update checks | `86400` |
| `update.auto-check` | Enable background update checks | `true` |

`skills.personal-registry` is the pre-rc4 spelling of `registry.url` and still
resolves; the canonical key wins when both are set.

## Managing Config

`akm config` with no arguments opens a **settings panel** on a terminal — a
single scrolling list grouped by section (personal registry, artifacts,
features, shared registries) where checkboxes toggle and text fields edit in
place, every change saved immediately. `v` on a shared registry checks it is
reachable; `a` adds one.

```bash
# Open the settings panel (or dump config when not a terminal)
akm config

# Dump all config as key = value lines (scriptable)
akm config --plain

# Get a specific value
akm config registry.url

# Set a value
akm config artifacts.auto-push false

# Add / remove a shared registry
akm config shared.acme git@github.com:acme/skills.git
akm config shared.acme ""
```

## Machine Layout

AKM follows [XDG Base Directory](https://specifications.freedesktop.org/basedir-spec/latest/)
conventions:

```
~/.local/bin/akm                        # CLI binary
~/.local/share/akm/                     # (XDG_DATA_HOME)
  ├── library/                          # the registry's git working tree — skills, agents, instructions
  ├── library.json                      # derived index of the specs above, rebuilt on every sync
  ├── local.json                        # this machine's core deviations
  ├── tools.json                        # harness definitions
  └── shell/akm-init.sh                 # shell integration
~/.config/akm/config.toml               # configuration (XDG_CONFIG_HOME)
~/.cache/akm/                           # ephemeral data (XDG_CACHE_HOME)
  ├── shared/<name>/                    # checkout of a shared registry — browsable, never mounted
  └── <project>-<ts>-<pid>/             # per-session staging dirs
~/.akm/
  └── artifacts/<repo>/                 # artifact dirs per project
```

### Key directories

- **`~/.local/share/akm/library/`** — the cold library, which is your personal registry's working tree. Editing a skill here is editing your registry.
- **`~/.local/share/akm/library.json`** — a derived, machine-local index, regenerated on every sync. Edit the per-spec `akm.json` sidecar, never this file.
- **`~/.config/akm/config.toml`** — the config file. TOML format.
- **`~/.cache/akm/`** — ephemeral data. Shared-registry checkouts under `shared/<name>/`, plus per-session staging directories created and destroyed by the shell wrappers.
- **`~/.akm/artifacts/`** — artifact directories, one per project.

Global instructions live inside the registry at `instructions/global.md`, not
under `~/.akm/` — see [Instructions](/docs/instructions/).
