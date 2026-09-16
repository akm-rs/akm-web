---
title: Skills
description: How AKM manages LLM agent skills — a library that is your registry's working tree.
---

## Overview

AKM maintains a **cold library** of skills on your machine. The library is the
git working tree of your personal registry, checked out at
`~/.local/share/akm/library/`. A skill you edit locally survives the next sync,
AKM can tell you which side has moved, and publishing one skill is one commit.

Skills are never all loaded globally. They activate through two layers that give
you precise control over which skills are available in each context.

Skills follow the [Agent Skills specification](https://agentskills.io/specification),
an open standard for portable AI coding skills.

## Two-Layer Activation

```
Layer 1 — Core (global, always available)
  Specs marked core in their akm.json sidecar
  Symlinked into ~/.claude/, ~/.copilot/, ~/.agents/, ~/.pi/agent/, ~/.vibe/,
  ~/.posit/assistant/

Layer 2 — Project (declared in a manifest, mounted per session)
  .agents/akm.json lists skill/agent IDs
  Shell wrapper reads the manifest → symlinks into a per-session staging dir
  akm skills add/remove refresh the running session immediately
```

### Layer 1 — Core

Core skills are globally available across all projects. Specs marked `core` in
their `akm.json` sidecar are symlinked directly into tool directories
(`~/.claude/`, `~/.copilot/`, `~/.agents/`, `~/.pi/agent/`, `~/.vibe/`,
`~/.posit/assistant/`). The
`core` default lives in the sidecar and propagates to your other machines;
a per-machine override stays in `local.json` and does not.

### Layer 2 — Project

Project-level skills are declared in a manifest file and loaded automatically
when you start a session. The shell wrappers read the manifest and symlink the
declared specs into a per-session staging directory. `akm skills add` and
`akm skills remove` update that manifest and refresh the active session's
symlinks straight away, so a skill added mid-session is available without a
restart.

## Shell Wrappers

`akm setup` wires `akm-init.sh` into your `.bashrc`, providing wrapper functions
for `claude`, `copilot`, `opencode`, `pi` and `vibe`. These wrappers handle the full
lifecycle:

1. **Pull** latest artifacts (if enabled)
2. **Create** a per-session skills staging directory with manifest specs loaded
3. **Hand** the staging and artifact dirs to the tool in the form it understands — `--add-dir` for Claude Code, Copilot and Mistral Vibe, `OPENCODE_CONFIG_DIR` for OpenCode, `--skill` for Pi
4. **Cleanup** on exit: destroy the staging dir, commit+push artifacts (if auto-push enabled)

Mistral Vibe (2.10.0+) reads `<dir>/.vibe/skills` from any `--add-dir`, so the
wrapper hands it the staging directory the same way it does Copilot. Core
skills and global instructions live in `~/.vibe/`. Vibe agents are TOML
configs rather than markdown personas, so agent specs are not mounted for it.

Posit Assistant runs inside the Positron IDE, so there is no command to wrap
and no session to hook into. Its skill discovery skips symlinked directories,
so core skills are mounted as real directories under
`~/.posit/assistant/skills/<id>/` whose entries are symlinks into the library.
Project skills are materialized into a gitignored sidecar at
`<project>/.posit/assistant/skills/`, refreshed by `akm skills add`/`remove`,
the interactive TUI, and at session setup. Posit scans skills when a
conversation starts, so reload the Positron window (or start a new
conversation) after syncing to see new skills.

## Project Manifests

Declare which skills a project uses in `.agents/akm.json`:

```json
{
  "skills": ["test-driven-development", "systematic-debugging"],
  "agents": ["code-reviewer"]
}
```

These are loaded automatically when you start a session via the shell wrappers.
Manage with:

```bash
# Add skills to the manifest (also loads them into the running session)
akm skills add test-driven-development systematic-debugging

# Remove a skill
akm skills remove systematic-debugging
```

## Managing Skills

### Browsing and searching

```bash
# Browse the library (also what bare `akm skills` opens)
akm skills list

# Filter by tag or type
akm skills list --tag testing
akm skills list --type agent

# Search by keyword
akm skills search debugging
```

Bare `akm skills` opens the library list — the same view as `akm skills list`.

### Full status overview

```bash
akm skills status
```

The status view leads with the **Manifest** section — the specs the current
project declares — since that is what matters most when standing in a project;
Core and the rest of the library follow. The `list` and `status` TUIs share a
set of action keys — `e` edits a spec, `a`
adds it to the project manifest, `r` removes it, `R` renames, `D` deletes. On a
spec that shows drift, `p` queues it to publish and `u` discards your local
edits. Add `--plain` to either command for scriptable plain-text output.

## The Registry Model

Your library **is** your personal registry's git working tree. There is one
writable registry — configured with `registry.url` — and it is the only one
mounted into tool directories. Read-only [shared registries](/docs/shared-registries/)
are troves you import from; nothing in them is ever mounted.

`akm skills sync` keeps the library in step with the registry:

1. **Fetch** the registry
2. **Fast-forward** the working tree — never a real merge, so no conflict marker
   can reach a skill symlinked live into a tool directory
3. An edit of yours to a skill the update also touched is **parked** and put back
   on top; everything else fast-forwards
4. **Regenerate** `library.json` and **rebuild** core symlinks

Sync never merges and never prompts — it reports what needs a decision, and you
decide with `publish`, `diff` or `revert`.

## Drift Markers

Because the library is a git working tree, AKM can tell you which side moved.
Each spec carries a marker in `akm skills list`, `akm skills status` and the
sync report:

| Marker | Meaning | What to do |
|--------|---------|-----------|
| `*` | Edited here, not yet published | `akm skills publish <id>` |
| `v` | The registry is ahead | `akm skills sync` |
| `!` | Both sides moved (diverged) | `akm skills diff <id>`, then publish or revert |

```bash
# Inspect a diverged spec
akm skills diff my-skill

# Keep your version
akm skills publish my-skill

# Discard your edits (locally, or take the registry's copy)
akm skills revert my-skill
akm skills revert my-skill --remote
```

## Publishing

Publishing writes to *your* registry on your own authority. One intent is one
commit:

```bash
# Publish a single spec
akm skills publish my-skill

# Publish everything pending, as one commit and one push
akm skills publish

# Preview either without touching the remote
akm skills publish --dry-run
```

When run interactively, `akm skills promote` and `akm skills import` offer to
publish the new skill right away. See [Skill Format](/docs/skill-format/) for
authoring and [Shared Registries](/docs/shared-registries/) for pulling from,
and contributing back to, other people's repos.

## Importing from GitHub

Import any skill directly from a GitHub repository URL:

```bash
# Import from a directory URL
akm skills import https://github.com/user/repo/tree/main/skills/my-skill

# Import with a custom ID
akm skills import https://github.com/user/repo/tree/main/skills/my-skill --id custom-name

# Import every skill under a directory
akm skills import https://github.com/user/repo/tree/main/skills --all
```

Both `/tree/` (directory) and `/blob/` (file) GitHub URLs are supported. For
private repos, set the `GITHUB_TOKEN` environment variable.
