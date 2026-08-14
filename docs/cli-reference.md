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
| `akm config` | Open the settings panel (TUI); dumps config when not a terminal |
| `akm config --plain` | Print all config as `key = value` lines |
| `akm config <key>` | Get a specific config value |
| `akm config <key> <value>` | Set a config value |
| `akm sync` | Sync all enabled domains |
| `akm update` | Check for and install the latest version |
| `akm completions <shell>` | Generate a shell completion script (bash, zsh, fish) |
| `akm help` | Show help |

## Skills

| Command | Description |
|---------|-------------|
| `akm skills` | Full status overview (defaults to `status`) |
| `akm skills sync` | Fast-forward the registry, rebuild the library and core symlinks |
| `akm skills add <id> [id...]` | Add spec(s) to the project manifest (refreshes the live session) |
| `akm skills remove <id> [id...]` | Remove spec(s) from the project manifest |
| `akm skills list` | Browse the library (interactive TUI) |
| `akm skills list --plain` | Browse the library (plain text) |
| `akm skills list --tag TAG` | Filter the library by tag |
| `akm skills list --type TYPE` | Filter the library by type |
| `akm skills list <registry>` | Browse a configured shared registry |
| `akm skills search <query>` | Search the library by keyword (TUI) |
| `akm skills search <query> --plain` | Search by keyword (plain text) |
| `akm skills status` | Full status overview (TUI) |
| `akm skills status --plain` | Full status overview (plain text) |
| `akm skills edit <id>` | Edit the spec's `SKILL.md` in `$EDITOR` |
| `akm skills edit <id> --meta` | Edit the spec's `akm.json` metadata sidecar |
| `akm skills rename <old> <new>` | Rename a spec's id (its slug / directory name) |
| `akm skills delete <id>` | Delete a spec from the library (and the personal registry) |
| `akm skills diff <id>` | Show what changed locally and on the registry |
| `akm skills revert <id>` | Discard local changes to a spec |
| `akm skills revert <id> --remote` | Replace the spec with the registry's copy |
| `akm skills core` | Show which specs are globally mounted |
| `akm skills core --adopt` | Adopt the registry's core flags for this machine |
| `akm skills core --publish` | Publish this machine's core flags for every machine |
| `akm skills promote <path>` | Import a local skill into cold storage |
| `akm skills promote <path> --force` | Import without overwrite confirmation |
| `akm skills import <github-url>` | Import a skill from a GitHub URL |
| `akm skills import <github-url> --all` | Import every skill a URL offers |
| `akm skills import <registry> <id>` | Import one skill from a shared registry |
| `akm skills import <registry> --all` | Import everything usable from a shared registry |
| `akm skills share <registry> <id>` | Offer a spec to a shared registry as a pull request |
| `akm skills publish <id>` | Publish one spec to the personal registry |
| `akm skills publish` | Publish every pending spec, in one commit |
| `akm skills publish --dry-run` | Preview a publish without pushing |
| `akm skills clean` | Remove stale specs |
| `akm skills clean --dry-run` | Preview what would be cleaned |
| `akm skills libgen` | Regenerate `library.json` from disk |

See [Shared Registries](/docs/shared-registries/) for `list`, `import` and `share` against a registry, and [Skills](/docs/skills/) for the drift markers (`*`, `v`, `!`).

## Artifacts

| Command | Description |
|---------|-------------|
| `akm artifacts` | Browse the artifacts tree (two-pane explorer) |
| `akm artifacts --plain` | Print the artifacts tree as plain text |
| `akm artifacts sync` | Bidirectional sync with the artifacts remote |

## Instructions

| Command | Description |
|---------|-------------|
| `akm instructions sync` | Distribute global instructions to all tool directories |
| `akm instructions edit` | Edit global instructions in `$EDITOR` |
| `akm instructions publish` | Publish global instructions to the personal registry |
| `akm instructions scaffold-project` | Create `AGENTS.md` + `CLAUDE.md` in the project root |

## Maintenance

| Command | Description |
|---------|-------------|
| `akm disable` | Turn off shell integration — new shells run harnesses vanilla (reversible) |
| `akm enable` | Re-enable shell integration after `akm disable` |
| `akm uninstall` | Remove akm — preserves artifacts and the library |
| `akm uninstall --purge` | Remove everything, including artifacts and the library |
| `akm uninstall --yes` | Skip the confirmation prompt |

## Examples

### Setting up a new project with skills

```bash
# Add skills to the project manifest — they load into the running session too
akm skills add test-driven-development systematic-debugging

# Check what's active
akm skills status
```

### Importing a skill from GitHub

```bash
# Import from a directory URL
akm skills import https://github.com/user/repo/tree/main/skills/my-skill

# Import with a custom ID
akm skills import https://github.com/user/repo/tree/main/skills/my-skill --id custom-name

# Import every skill under a directory
akm skills import https://github.com/user/repo/tree/main/skills --all

# For private repos, set GITHUB_TOKEN
GITHUB_TOKEN=ghp_... akm skills import https://github.com/user/repo/tree/main/skills/my-skill
```

### Promoting a local skill and publishing it

```bash
# Import a project-local skill into cold storage (prompts for description, tags, core flag)
akm skills promote ./my-skill

# Preview the publish
akm skills publish my-skill --dry-run

# Publish it — or publish everything pending in one commit
akm skills publish my-skill
akm skills publish
```

### Handling drift

```bash
# See what moved on a spec marked * or !
akm skills diff my-skill

# Keep your version and publish it
akm skills publish my-skill

# Or discard your edits — locally, or take the registry's copy
akm skills revert my-skill
akm skills revert my-skill --remote
```
