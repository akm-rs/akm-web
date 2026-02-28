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

Example:

```yaml
---
name: Test-Driven Development
description: Use when implementing any feature or bugfix, before writing implementation code
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

## Publishing Skills

To publish a project-local skill to your skills remote:

```bash
akm skills publish my-skill
```

This does the following:

1. Validates the skill's frontmatter
2. Copies the spec to a `publish/my-skill` branch in the cached skills remote
3. Commits and pushes the branch
4. Opens a PR via `gh`

### Options

| Flag | Description |
|------|-------------|
| `--dry-run` | Preview what would be published without pushing |
| `--force` | Overwrite an existing spec on the remote |

### Example workflow

```bash
# Preview first
akm skills publish my-skill --dry-run

# Publish for real
akm skills publish my-skill

# Force overwrite if the skill already exists
akm skills publish my-skill --force
```

## Agent Skills Specification

AKM skills conform to the open [Agent Skills specification](https://agentskills.io/specification). This standard ensures skills are portable across different tools and ecosystems.

## Related

- [Skills domain](/docs/skills/) -- Three-layer activation and skill management
- [CLI Reference](/docs/cli-reference/) -- Full command reference for `akm skills`
