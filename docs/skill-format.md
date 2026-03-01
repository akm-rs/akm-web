---
title: Skill Format
description: How to write and publish AKM skills.
---

## Directory Structure

Skills follow the [Agent Skills specification](https://agentskills.io/specification):

```
skills/<name>/
├── SKILL.md           # Entry point (YAML frontmatter + instructions)
└── references/        # Optional supporting files
```

Each skill is a directory containing a `SKILL.md` file as the entry point, plus an optional `references/` directory for supporting files.

## YAML Frontmatter

The `SKILL.md` file requires YAML frontmatter with two fields:

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Human-readable skill name |
| `description` | Yes | What the skill does (convention: starts with "Use when...") |
| `tags` | No | List of tags for filtering (e.g., `[testing, development]`) |

Example:

```yaml
---
name: Test-Driven Development
description: Use when implementing any feature or bugfix, before writing implementation code
tags: [testing, development]
---

## Instructions

Write tests first, then implement code to make them pass.
...
```

The body of `SKILL.md` (after the frontmatter) contains the actual instructions that will be provided to the LLM agent.

## Agent Format

Agents are simpler -- they are single `.md` files placed in the `agents/` directory with the same frontmatter format:

```
agents/
└── code-reviewer.md   # Single file with YAML frontmatter + instructions
```

Agents use the same `name` and `description` frontmatter fields as skills.

## Promoting and Publishing Skills

### Promote: Import a local skill into cold storage

Use `akm skills promote` to import a project-local skill into your cold library:

```bash
akm skills promote ./my-skill
```

You'll be prompted interactively for description, tags, and whether to mark the skill as core (globally available). Use `--force` to skip overwrite confirmation if the skill already exists.

### Publish: Push from cold storage to your personal registry

Use `akm skills publish` to push a skill from cold storage to your personal registry:

```bash
akm skills publish my-skill
```

This does the following:

1. Validates the spec exists in cold storage
2. Copies it to the cached personal registry
3. Regenerates `library.json` in the registry
4. Commits and pushes to the personal registry remote

Requires `SKILLS_PERSONAL_REGISTRY` to be configured (run `akm setup --skills`).

| Flag | Description |
|------|-------------|
| `--dry-run` | Preview what would be published without pushing |

### Example workflow

```bash
# Import a local skill into cold storage
akm skills promote ./my-skill

# Preview what would be published
akm skills publish my-skill --dry-run

# Publish for real
akm skills publish my-skill
```

## Agent Skills Specification

AKM skills conform to the open [Agent Skills specification](https://agentskills.io/specification). This standard ensures skills are portable across different tools and ecosystems.

## Related

- [Skills domain](/docs/skills/) -- Three-layer activation and skill management
- [CLI Reference](/docs/cli-reference/) -- Full command reference for `akm skills`
