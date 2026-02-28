---
title: Instructions
description: Write global LLM instructions once, distribute to all tool directories.
---

## Global Instructions

AKM lets you maintain a single set of global LLM instructions that are automatically distributed to all supported tool directories. Your instructions live in:

```
~/.akm/global-instructions.md
```

Write your instructions once, and AKM ensures every coding assistant sees them.

## Target Mapping

AKM distributes your global instructions to the appropriate directories and filenames for each supported tool. The mapping is defined in `tools.json`:

| Tool | Directory | Filename |
|------|-----------|----------|
| Claude Code | `~/.claude/` | `CLAUDE.md` |
| GitHub Copilot | `~/.copilot/` | `copilot-instructions.md` |
| OpenCode | `~/.agents/` | `AGENTS.md` |
| Mistral Vibe | `~/.vibe/` | `VIBE.md` |

## Commands

### Edit instructions

Open your global instructions in your default editor:

```bash
akm instructions edit
```

This opens `~/.akm/global-instructions.md` in `$EDITOR`.

### Sync instructions

Distribute your global instructions to all tool directories:

```bash
akm instructions sync
```

This copies the contents of your global instructions file to each tool's directory with the appropriate filename.

### Scaffold project

Create project-level instruction files in the current project root:

```bash
akm instructions scaffold-project
```

This creates `AGENTS.md` and `CLAUDE.md` in the current directory, providing a starting point for project-specific instructions.

## Workflow

1. Write your global instructions once: `akm instructions edit`
2. Sync to all tools: `akm instructions sync`
3. Optionally, scaffold project-level instructions: `akm instructions scaffold-project`

Instructions sync is also triggered automatically by `akm sync`, which syncs all enabled domains.
