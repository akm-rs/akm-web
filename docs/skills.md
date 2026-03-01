---
title: Skills
description: How AKM manages LLM agent skills with three-layer activation.
---

## Overview

AKM maintains a **cold library** of installed skills on your machine. Skills are never loaded globally by default -- they are activated through a three-layer system that gives you precise control over which skills are available in each context.

Skills follow the [Agent Skills specification](https://agentskills.io/specification), an open standard for portable AI coding skills.

## Three-Layer Activation

```
Layer 1 — Core (global, always available)
  Specs marked core=true in library.json
  Symlinked into ~/.claude/, ~/.copilot/, ~/.agents/, ~/.vibe/

Layer 2 — Project (declared in manifest, loaded at session start)
  .agents/akm.json lists skill/agent IDs
  Shell wrapper reads manifest → symlinks into per-session staging dir

Layer 3 — Session (JIT, mid-session)
  akm skills load <id>   → adds to active staging dir
  akm skills unload <id> → removes from staging dir
```

### Layer 1 -- Core

Core skills are globally available across all projects. Specs marked `core=true` in `library.json` are symlinked directly into tool directories (`~/.claude/`, `~/.copilot/`, `~/.agents/`, `~/.vibe/`).

### Layer 2 -- Project

Project-level skills are declared in a manifest file and loaded automatically when you start a session. The shell wrappers read the manifest and symlink the declared specs into a per-session staging directory.

### Layer 3 -- Session

Session-level skills are loaded on demand during an active session. Use `akm skills load` and `akm skills unload` for just-in-time activation.

## Shell Wrappers

`akm setup` wires `akm-init.sh` into your `.bashrc`, providing wrapper functions for `claude`, `copilot`, `vibe`, and `opencode`. These wrappers handle the full lifecycle:

1. **Pull** latest artifacts (if enabled)
2. **Create** a per-session skills staging directory with manifest specs loaded
3. **Pass** artifact and staging dirs to the tool via `--add-dir`
4. **Cleanup** on exit: destroy staging dir, commit+push artifacts (if auto-push enabled)

## Project Manifests

Declare which skills a project uses in `.agents/akm.json`:

```json
{
  "skills": ["test-driven-development", "systematic-debugging"],
  "agents": ["code-reviewer"]
}
```

These are loaded automatically when you start a session via the shell wrappers. Manage with:

```bash
# Add skills to the manifest
akm skills add test-driven-development systematic-debugging

# Remove a skill
akm skills remove systematic-debugging
```

## Managing Skills

### Browsing and searching

```bash
# List all installed skills
akm skills list

# Filter by tag or type
akm skills list --tag testing
akm skills list --type agent

# Search by keyword
akm skills search debugging
```

### Loading and unloading (session)

```bash
# Load a skill mid-session
akm skills load code-reviewer

# Check what's loaded and where it came from
akm skills loaded

# Unload when done
akm skills unload code-reviewer
```

### Full status overview

```bash
akm skills status
```

## Dual Registry Model

AKM supports two independent registries:

- **Community registry** (`skills.community-registry`): Where you *pull* skills from (read-only source, defaults to [Skillverse](https://github.com/akm-rs/skillverse))
- **Personal registry** (`skills.personal-registry`): Where you *push* your own skills to (read-write publish target)

These are independent and can be different repos.

## Sync Pipeline

`akm skills sync` runs the full sync pipeline:

1. **Pull** community registry (e.g., Skillverse) to cache
2. **Copy** community registry to cold library (clean slate)
3. **Pull** personal registry to cache
4. **Overlay** personal registry onto cold library (personal wins on conflict)
5. **Regenerate** `library.json` from disk
6. **Rebuild** core symlinks across all tool directories

This keeps your local library up to date with both registries.

## Promoting and Publishing

Import a project-local skill into cold storage, then publish to your personal registry:

```bash
# Import local skill into cold storage
akm skills promote ./my-skill

# Publish from cold storage to personal registry
akm skills publish my-skill
```
