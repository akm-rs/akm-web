---
title: Getting Started
description: Install and configure AKM in under a minute.
---

## Prerequisites

AKM requires two tools:

- **jq** -- JSON processor
  ```bash
  # Debian/Ubuntu
  sudo apt-get install jq

  # macOS
  brew install jq
  ```

- **git** -- For skills sync, repo detection, and artifact sync

## Install

Clone the repository and run the installer:

```bash
git clone https://github.com/akm-rs/akm ~/akm
bash ~/akm/install.sh
```

The installer places the `akm` binary in `~/.local/bin/` and the shell integration in `~/.local/share/akm/shell/`.

## Setup

Run the interactive setup wizard:

```bash
akm setup
```

Setup asks three things:

1. **Enable skills?** (Y) -- Installs the cold library and sets up three-layer activation
2. **Use Skillverse?** (Y) -- Configures [Skillverse](https://github.com/akm-rs/skillverse) as your default skills remote
3. **Enable artifacts/instructions?** -- Configure artifact sync and global instructions

The happy path is Enter through everything -- defaults work out of the box.

## Verify

Open a new terminal (or `source ~/.bashrc`), then check your installation:

```bash
# Full status overview
akm skills status

# Browse installed skills
akm skills list
```

## Shell Wrappers

`akm setup` wires `akm-init.sh` into your `.bashrc`, which provides wrapper functions for `claude`, `copilot`, and `opencode`. These wrappers automatically:

1. Pull latest artifacts (if enabled)
2. Create a per-session skills staging directory with manifest specs loaded
3. Pass artifact and staging dirs to the tool via `--add-dir`
4. On exit: destroy staging dir, commit+push artifacts (if auto-push enabled)

This means you just type `claude` or `copilot` as usual -- AKM handles skills and artifacts transparently.

## Next Steps

- [Skills](/docs/skills/) -- Learn about three-layer activation and managing skills
- [CLI Reference](/docs/cli-reference/) -- Complete command reference
- [Configuration](/docs/configuration/) -- Customize AKM settings
