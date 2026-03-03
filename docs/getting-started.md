---
title: Getting Started
description: Install and configure AKM in under a minute.
---

## Prerequisites

AKM requires one runtime dependency:

- **git** — For skills sync, repo detection, and artifact sync

## Install

### Quick install (Linux x86_64)

Download the latest release binary:

```bash
curl -fsSL https://raw.githubusercontent.com/akm-rs/akm-rs/main/scripts/install.sh | bash
```

This downloads the latest release binary to `~/.local/bin/akm`.

Options:

```bash
# Install a specific version
AKM_VERSION=1.0.0 curl -fsSL https://raw.githubusercontent.com/akm-rs/akm-rs/main/scripts/install.sh | bash

# Install to a custom directory
AKM_INSTALL_DIR=/usr/local/bin curl -fsSL https://raw.githubusercontent.com/akm-rs/akm-rs/main/scripts/install.sh | bash
```

### From crates.io

If you have Rust installed:

```bash
cargo install akm
```

### From source

```bash
git clone https://github.com/akm-rs/akm-rs.git
cd akm-rs
cargo install --path .
```

## Setup

Run the interactive setup wizard:

```bash
akm setup
```

Setup asks about each domain:

1. **Enable skills?** (Y) -- Installs the cold library and sets up three-layer activation
2. **Use Skillverse?** (Y) -- Configures [Skillverse](https://github.com/akm-rs/skillverse) as your community registry
3. **Personal registry?** -- Optionally configure a personal registry for publishing your own skills
4. **Enable artifacts/instructions?** -- Configure artifact sync and global instructions

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

`akm setup` wires `akm-init.sh` into your `.bashrc`, which provides wrapper functions for `claude`, `copilot`, `vibe`, and `opencode`. These wrappers automatically:

1. Pull latest artifacts (if enabled)
2. Create a per-session skills staging directory with manifest specs loaded
3. Pass artifact and staging dirs to the tool via `--add-dir`
4. On exit: destroy staging dir, commit+push artifacts (if auto-push enabled)

This means you just type `claude` or `copilot` as usual -- AKM handles skills and artifacts transparently.

## Next Steps

- [Skills](/docs/skills/) -- Learn about three-layer activation and managing skills
- [CLI Reference](/docs/cli-reference/) -- Complete command reference
- [Configuration](/docs/configuration/) -- Customize AKM settings
