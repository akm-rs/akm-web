---
title: Instructions
description: Write global LLM instructions once, distribute to all tool directories.
---

## Global Instructions

AKM lets you maintain a single set of global LLM instructions that are
automatically distributed to all supported tool directories. Your instructions
live inside your registry, at:

```
~/.local/share/akm/library/instructions/global.md
```

Because they live in the registry, global instructions gain the same drift,
publish and propagation model as a skill — edit them on one machine, publish,
and they follow you to the next.

A pre-rc4 `~/.akm/global-instructions.md` is carried into the registry the first
time the new file is needed; the old file is left where it is.

## Target Mapping

Each tool gets the instructions under the directory and filename it expects:

| Tool | Directory | Filename |
|------|-----------|----------|
| Claude Code | `~/.claude/` | `CLAUDE.md` |
| GitHub Copilot | `~/.copilot/` | `copilot-instructions.md` |
| OpenCode | `~/.agents/` | `AGENTS.md` |
| Pi | `~/.pi/agent/` | `AGENTS.md` |
| Mistral Vibe | `~/.vibe/prompts/` | `cli.md` |

## Commands

### Edit instructions

Open your global instructions in your default editor:

```bash
akm instructions edit
```

### Sync instructions

Distribute your global instructions to all tool directories:

```bash
akm instructions sync
```

This writes the instructions file out under each tool's expected name. It is
also triggered automatically by `akm sync`, which syncs all enabled domains.

### Publish instructions

Push your global instructions to the personal registry so they propagate to your
other machines:

```bash
akm instructions publish
```

### Scaffold project

Create project-level instruction files in the current project root:

```bash
akm instructions scaffold-project
```

This creates `AGENTS.md` and `CLAUDE.md` in the current directory.

## Workflow

1. Write your global instructions once: `akm instructions edit`
2. Distribute them to all tools: `akm instructions sync`
3. Propagate them to your other machines: `akm instructions publish`
4. Optionally, scaffold project-level instructions: `akm instructions scaffold-project`
