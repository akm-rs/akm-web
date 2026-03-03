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
| `akm update` | Check for and install latest version |
| `akm completions <shell>` | Generate shell completion script (bash, zsh, fish) |
| `akm help` | Show help |

## Skills

| Command | Description |
|---------|-------------|
| `akm skills sync` | Pull remote, update cold library, rebuild symlinks |
| `akm skills sync --quiet` | Sync with suppressed output |
| `akm skills add <id> [id...]` | Add spec(s) to the project manifest |
| `akm skills remove <id> [id...]` | Remove spec(s) from the project manifest |
| `akm skills load <id> [id...]` | Load spec(s) into the active session (JIT) |
| `akm skills unload <id> [id...]` | Remove spec(s) from the active session |
| `akm skills loaded` | Show active session specs with provenance |
| `akm skills list` | Browse library (interactive TUI) |
| `akm skills list --plain` | Browse library (plain text output) |
| `akm skills list --tag TAG` | Filter library by tag |
| `akm skills list --type TYPE` | Filter library by type |
| `akm skills search <query>` | Search by keyword (interactive TUI) |
| `akm skills search <query> --plain` | Search by keyword (plain text) |
| `akm skills status` | Full status overview (interactive TUI) |
| `akm skills status --plain` | Full status overview (plain text) |
| `akm skills clean` | Remove stale specs |
| `akm skills clean --project` | Clean project-level specs |
| `akm skills clean --dry-run` | Preview what would be cleaned |
| `akm skills promote <path>` | Import a local skill into cold storage |
| `akm skills promote <path> --force` | Import without overwrite confirmation |
| `akm skills edit <id>` | Edit spec metadata in `$EDITOR` |
| `akm skills publish <id>` | Publish spec from cold storage to personal registry |
| `akm skills publish <id> --dry-run` | Preview publish without pushing |
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

### Promoting a local skill to cold storage

```bash
# Import a project-local skill into cold storage
akm skills promote ./my-skill

# You'll be prompted for description, tags, and core flag
```

### Publishing a skill to your personal registry

```bash
# Preview first
akm skills publish my-skill --dry-run

# Publish from cold storage to personal registry
akm skills publish my-skill
```
