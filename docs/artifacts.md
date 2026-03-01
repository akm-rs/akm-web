---
title: Artifacts
description: Auto-sync LLM session outputs to a git repository.
---

## What Artifacts Are

Artifacts are LLM session outputs -- plans, research notes, design documents, session summaries -- that you want to persist across sessions. AKM syncs these to a dedicated git repository, so your work products are versioned and accessible from any machine.

## Setup

Enable artifacts during `akm setup`:

```bash
akm setup --artifacts
```

You will be asked to provide a git remote URL for your artifacts repository:

```
git@github.com:<user>/<artifacts-repo>.git
```

The local artifacts directory defaults to `~/.akm/artifacts/`.

## How It Works

AKM provides **bidirectional sync** between your local artifacts directory and the remote git repository:

- **On session start** (via shell wrappers): pull latest artifacts from the remote
- **On session exit**: commit and push any new or changed artifacts

This happens transparently when you use the shell wrappers (`claude`, `copilot`, `vibe`, `opencode`).

## Auto-Push

The `ARTIFACTS_AUTO_PUSH` config key controls whether artifacts are automatically committed and pushed when a session exits:

```bash
# Enable auto-push (default)
akm config artifacts.auto-push true

# Disable auto-push
akm config artifacts.auto-push false
```

When auto-push is enabled, the shell wrapper will commit and push artifacts on session exit without any manual intervention.

## Manual Sync

You can manually sync artifacts at any time:

```bash
akm artifacts sync
```

This performs a bidirectional sync: pulls from the remote, then commits and pushes local changes.

## Configuration Keys

| Key | Description | Default |
|-----|-------------|---------|
| `artifacts.remote` | Git remote URL for artifacts repo | *(set during setup)* |
| `artifacts.dir` | Local artifacts directory | `~/.akm/artifacts` |
| `artifacts.auto-push` | Auto commit+push on session exit | `true` |

See [Configuration](/docs/configuration/) for details on managing all config keys.
