---
title: Configuration
description: AKM configuration reference and machine layout.
---

## Config File

All AKM configuration lives in a single TOML file:

```
~/.config/akm/config.toml
```

The file uses [TOML](https://toml.io/) format and is created by `akm setup` or on first run with defaults:

```toml
features = ["skills", "artifacts", "instructions"]

[skills]
community_registry = "https://github.com/akm-rs/skillverse.git"
personal_registry = "git@github.com:<user>/<my-skills>.git"

[artifacts]
remote = "git@github.com:<user>/<artifacts-repo>.git"
dir = "~/.akm/artifacts"
auto_push = true
```

## Config Keys

| Key | Description | Default |
|-----|-------------|---------|
| `features` | Enabled domains (comma-separated: `skills`, `artifacts`, `instructions`) | `skills` |
| `skills.community_registry` | Git remote for community skills (read-only source) | [Skillverse](https://github.com/akm-rs/skillverse) |
| `skills.personal_registry` | Git remote for your own skills (read-write publish target) | *(set during setup)* |
| `artifacts.remote` | Git remote for artifacts repo | *(set during setup)* |
| `artifacts.dir` | Local artifacts directory | `~/.akm/artifacts` |
| `artifacts.auto_push` | Auto commit+push artifacts on session exit | `true` |

## Managing Config

Use the `akm config` command:

```bash
# View all configuration
akm config

# Get a specific value
akm config skills.community_registry

# Set a value
akm config artifacts.auto_push false
```

## Machine Layout

AKM follows [XDG Base Directory](https://specifications.freedesktop.org/basedir-spec/latest/) conventions:

```
~/.local/bin/akm                        # CLI binary
~/.local/share/akm/                     # Cold library (XDG_DATA_HOME)
  ├── skills/                           # Installed skills
  ├── agents/                           # Installed agents
  ├── library.json                      # Generated spec registry
  ├── tools.json                        # Tool directory mappings
  └── shell/akm-init.sh                # Shell integration
~/.config/akm/config.toml              # Configuration (XDG_CONFIG_HOME)
~/.cache/akm/                           # Ephemeral data (XDG_CACHE_HOME)
  ├── skills-community-registry/       # Cached clone of community registry
  ├── skills-personal-registry/        # Cached clone of personal registry
  └── <project>-<ts>-<pid>/            # Session staging dirs
~/.akm/
  ├── global-instructions.md            # Global LLM instructions
  └── artifacts/<repo>/                 # Artifact dirs per project
```

### Key directories

- **`~/.local/share/akm/`** -- The cold library. Contains all installed skills and agents, plus the generated `library.json` registry.
- **`~/.config/akm/config.toml`** -- The config file. TOML format.
- **`~/.cache/akm/`** -- Ephemeral data. Cached clones of the community and personal registries, plus per-session staging directories (created and destroyed by shell wrappers).
- **`~/.akm/`** -- User-facing data. Global instructions and artifact directories.
