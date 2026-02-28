---
title: CLI Reference
description: Complete reference for all AKM commands and subcommands.
---

Usage:

```
akm <command> [subcommand] [options]
```

## Setup and Config

| Command | Description |
|---------|-------------|
| `akm setup` | Interactive configuration wizard |
| `akm setup --skills` | Configure only the skills domain |
| `akm setup --artifacts` | Configure only the artifacts domain |
| `akm setup --instructions` | Configure only the instructions domain |
| `akm config` | View all configuration |
| `akm config <key>` | Get a specific config value |
| `akm config <key> <value>` | Set a config value |
| `akm sync` | Sync all enabled domains |
| `akm help` | Show help |

## Skills

| Command | Description |
|---------|-------------|
| `akm skills sync` | Pull remote, update cold library, rebuild symlinks |
| `akm skills add <id> [id...]` | Add spec(s) to the project manifest |
| `akm skills remove <id> [id...]` | Remove spec(s) from the project manifest |
| `akm skills load <id> [id...]` | Load spec(s) into the active session (JIT) |
| `akm skills unload <id> [id...]` | Remove spec(s) from the active session |
| `akm skills loaded` | Show active session specs with provenance |
| `akm skills list` | Browse library |
| `akm skills list --tag TAG` | Filter library by tag |
| `akm skills list --type TYPE` | Filter library by type |
| `akm skills search <query>` | Search by keyword |
| `akm skills status` | Full status overview |
| `akm skills clean` | Remove stale specs |
| `akm skills clean --project` | Clean project-level specs |
| `akm skills clean --dry-run` | Preview what would be cleaned |
| `akm skills clean --project --migrate` | Migrate legacy copies to manifest |
| `akm skills publish <id>` | Publish spec as PR to remote |
| `akm skills publish <id> --dry-run` | Preview publish without pushing |
| `akm skills publish <id> --force` | Overwrite existing spec on remote |
| `akm skills libgen` | Regenerate library.json from disk |

## Artifacts

| Command | Description |
|---------|-------------|
| `akm artifacts sync` | Bidirectional sync with artifacts remote |

## Instructions

| Command | Description |
|---------|-------------|
| `akm instructions edit` | Edit global instructions in `$EDITOR` |
| `akm instructions sync` | Distribute instructions to all tool directories |
| `akm instructions scaffold-project` | Create AGENTS.md + CLAUDE.md in project root |

## Examples

### Setting up a new project with skills

```bash
# Add skills to your project manifest
akm skills add test-driven-development systematic-debugging

# Check what's loaded
akm skills loaded
```

### Mid-session skill loading

```bash
# Load a skill on the fly
akm skills load code-reviewer

# When done, unload it
akm skills unload code-reviewer
```

### Publishing a skill

```bash
# Preview first
akm skills publish my-skill --dry-run

# Then publish
akm skills publish my-skill
```
