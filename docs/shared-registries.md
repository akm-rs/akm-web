---
title: Shared Registries
description: Browse, import from, and contribute back to other people's skill repositories.
---

## What a Shared Registry Is

A shared registry is somebody else's skills repository — a team's, a colleague's,
a community's. AKM treats it as a **trove to pick from, not a second library**:
nothing in it is ever mounted into a tool directory. You browse it, take what you
want, and the copy becomes an ordinary skill of your own.

This is the key distinction from your **personal registry** (`registry.url`),
which is writable and *is* your library. Shared registries are read-only sources.

## Adding a Registry

Add shared registries in the settings panel, or scriptably:

```bash
akm config                                              # settings panel: add / remove / verify

akm config shared.acme git@github.com:acme/skills.git   # add one, scriptably
akm config shared.acme ""                               # remove it, scriptably
```

Both write the same `[shared]` config. In the settings panel, `v` on a shared
registry checks it is reachable and `a` adds one.

Checkouts live in `~/.cache/akm/shared/<name>/` and are refreshed by
`akm skills sync` and by the commands below. Removing a registry deletes its
checkout too. A registry that cannot be reached is reported, and browsing falls
back to the copy already on disk.

## Browsing

```bash
akm skills list acme      # what it offers, marking what you already have
```

In the interactive `akm skills list` TUI, a configured shared registry appears as
a tab. The tab bar shows `Library` first, then one read-only tab per registry,
switched with `Tab` / `Shift+Tab`. On a shared tab: `r` fetches it, `Enter`
previews a candidate's `SKILL.md`, and `i` imports the selected skill. With no
shared registry configured there is no tab bar.

## Importing

```bash
akm skills import acme tdd     # take one skill
akm skills import acme --all   # take everything usable
```

Imported skills land in your library as ordinary specs. When run interactively,
the import is offered for publishing to your personal registry afterwards.

A registry can be laid out either way — skills under a `skills/` directory, or one
directory per skill at the repo root. AKM detects which. Only directories holding
a `SKILL.md` with a `name` and a `description` are importable.

### Conflicts

An id you already have is a conflict. Interactively, you choose per skill:

```
  tdd — already in your library, and it differs.
    [m]ine  [t]heirs  [b]oth as 'acme-tdd'  (add 'a' for all):
```

`both` keeps yours and stores theirs under the prefixed id. Without a terminal,
`--all` keeps yours and says so, while a single import fails unless you pass
`--force`. Identical content is skipped silently, so re-running an import is a
no-op until the registry actually moves.

A registry's `core: true` is never inherited — that flag is its owner's statement
about their machines. Imported skills land unmounted; make one core here with
`akm skills edit <id> --meta`.

## Contributing Back

```bash
akm skills share acme my-skill
```

This pushes the spec to the shared registry on branch `akm/<id>` and relays the
URL the remote prints for opening a pull request. Nothing is merged — the
registry's owners decide. It needs permission to push a branch, not to write to
the default branch, and re-running it updates the same branch so an open pull
request follows along.

`publish` and `share` are not the same act: **`publish`** writes to *your*
registry on your own authority; **`share`** offers a copy to someone else's and
waits for their review.

## Related

- [Skills](/docs/skills/) -- The library, the registry model, and drift markers
- [Configuration](/docs/configuration/) -- The `[shared]` config section
