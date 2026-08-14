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

## YAML Frontmatter

`SKILL.md` requires YAML frontmatter with two fields:

| Field | Description |
|-------|-------------|
| `name` | Human-readable skill name |
| `description` | What the skill does (convention: starts with "Use when...") |

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

The body of `SKILL.md`, after the frontmatter, is what the agent reads.

Tags and the core flag are not frontmatter — they live in a per-spec `akm.json`
sidecar beside `SKILL.md`. They are set when you promote or import a skill, or
later with `akm skills edit <id> --meta`. `library.json` is a derived,
machine-local index built from these sidecars on every sync — never edit it by
hand.

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

Because the library is the registry's working tree, publishing is a commit and a
push. One intent is one commit — `akm skills publish <id>` sends a single spec,
and `akm skills publish` with no id sends every pending spec at once. If the
remote moved on since your last sync, AKM fast-forwards onto it first so the push
is never rejected.

Requires `registry.url` to be configured (run `akm setup --skills`).

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

## Related

- [Skills domain](/docs/skills/) -- Two-layer activation, the registry model, and drift markers
- [Shared Registries](/docs/shared-registries/) -- Import from, and contribute back to, other people's repos
- [CLI Reference](/docs/cli-reference/) -- Full command reference for `akm skills`
