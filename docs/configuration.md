---
title: Configuration
description: AKM configuration reference and machine layout.
---

## Config File

All AKM configuration lives in a single flat file:

```
~/.config/akm/config
```

The file uses `key=value` format and is directly sourceable by bash:

```bash
FEATURES="skills,artifacts,instructions"
SKILLS_REMOTE="https://github.com/akm-rs/skillverse.git"
ARTIFACTS_REMOTE="git@github.com:<user>/<artifacts-repo>.git"
ARTIFACTS_DIR="$HOME/.akm/artifacts"
ARTIFACTS_AUTO_PUSH="true"
```

## Config Keys

| Key | Description | Default |
|-----|-------------|---------|
| `features` | Enabled domains (comma-separated: `skills`, `artifacts`, `instructions`) | `skills` |
| `skills.remote` | Git remote for skills library | [Skillverse](https://github.com/akm-rs/skillverse) |
| `artifacts.remote` | Git remote for artifacts repo | *(set during setup)* |
| `artifacts.dir` | Local artifacts directory | `~/.akm/artifacts` |
| `artifacts.auto-push` | Auto commit+push artifacts on session exit | `true` |

## Managing Config

Use the `akm config` command:

```bash
# View all configuration
akm config

# Get a specific value
akm config skills.remote

# Set a value
akm config artifacts.auto-push false
```

## Machine Layout

AKM follows [XDG Base Directory](https://specifications.freedesktop.org/basedir-spec/latest/) conventions:

```
~/.local/bin/akm                        # CLI binary
~/.local/share/akm/                     # Cold library (XDG_DATA_HOME)
  ├── skills/                           # Installed skills
  ├── agents/                           # Installed agents
  ├── library.json                      # Generated spec registry
  └── shell/akm-init.sh                # Shell integration
~/.config/akm/config                    # Configuration (XDG_CONFIG_HOME)
~/.cache/akm/                           # Ephemeral data (XDG_CACHE_HOME)
  ├── skills-remote/                    # Cached clone of skills remote
  └── <project>-<ts>-<pid>/            # Session staging dirs
~/.akm/
  ├── global-instructions.md            # Global LLM instructions
  └── artifacts/<repo>/                 # Artifact dirs per project
```

### Key directories

- **`~/.local/share/akm/`** -- The cold library. Contains all installed skills and agents, plus the generated `library.json` registry.
- **`~/.config/akm/config`** -- The config file. Flat key=value format.
- **`~/.cache/akm/`** -- Ephemeral data. Cached clone of the skills remote and per-session staging directories (created and destroyed by shell wrappers).
- **`~/.akm/`** -- User-facing data. Global instructions and artifact directories.
