---
title: Getting Started
description: Install and configure AKM in under a minute.
---

## Prerequisites

AKM requires one runtime dependency:

- **git** — For skills sync, repo detection, and artifact sync

## Install

### Quick install (Linux x86_64 / macOS ARM)

```bash
curl -fsSL https://akm.raphaelsimon.fr/install | sh
```

Installs the latest release binary to `~/.local/bin/akm`, detecting the platform:

| Platform | Architecture | Asset |
|----------|-------------|-------|
| Linux | x86_64 | `akm-linux-x86_64` (static, musl) |
| macOS | Apple Silicon (M1+) | `akm-macos-aarch64` |

Other platforms (Linux ARM, Intel Mac) can install via `cargo install akm`.

Options:

```bash
# Install a specific version
AKM_VERSION=1.0.0 curl -fsSL https://akm.raphaelsimon.fr/install | sh

# Install to a custom directory
AKM_INSTALL_DIR=/usr/local/bin curl -fsSL https://akm.raphaelsimon.fr/install | sh
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

Setup walks through each domain and wires shell integration into your `.bashrc`:

1. **Skills** -- installs the cold library and sets up core + project activation. Point it at your personal registry (`registry.url`) — the git repo that *is* your library — for sync and publishing.
2. **Artifacts** -- configure a git remote to persist session outputs (plans, research, notes).
3. **Instructions** -- enable global instruction distribution to every tool directory.

The happy path is Enter through everything -- defaults work out of the box. You
can add read-only [shared registries](/docs/shared-registries/) to import from
at any time, later.

## Verify

Open a new terminal (or `source ~/.bashrc`), then check your installation:

```bash
# Full status overview
akm skills status

# Browse installed skills
akm skills list
```

## Shell Wrappers

`akm setup` wires `akm-init.sh` into your `.bashrc`, which provides wrapper functions for `claude`, `copilot`, `opencode`, `pi` and `vibe`. These wrappers automatically:

1. Pull latest artifacts (if enabled)
2. Create a per-session skills staging directory with manifest specs loaded
3. Hand the staging and artifact dirs to the tool in the form it understands (`--add-dir`, `OPENCODE_CONFIG_DIR`, or `--skill`)
4. On exit: destroy staging dir, commit+push artifacts (if auto-push enabled)

You just type `claude`, `copilot`, `opencode`, `pi` or `vibe` as usual.

## Next Steps

- [Skills](/docs/skills/) -- Learn about two-layer activation and managing skills
- [CLI Reference](/docs/cli-reference/) -- Complete command reference
- [Configuration](/docs/configuration/) -- Customize AKM settings
