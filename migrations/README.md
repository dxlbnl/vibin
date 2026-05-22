# Migrations

Vibin is a **seed repo**, cloned per project. Improvements to the seed (its agents, skills,
hooks, operating rules, and wiki templates) therefore do **not** reach already-bootstrapped
projects automatically. This directory makes those upgrades **executable**: one structured,
agent-runnable file per released seed change.

Use [`/migrate-vibin`](../.claude/skills/migrate-vibin/SKILL.md) to apply pending migrations to an
existing project. `CHANGELOG.md` (repo root) is the human-facing index of the same changes.

## Versioning

- The repo root holds **`.vibin-version`** — the **git commit hash** of the Vibin seed a
  project is currently synced to. `/bootstrap` stamps it with the commit the project was
  cloned from; `/migrate-vibin` updates it after a successful upgrade.
- `/migrate-vibin` diffs that hash against the latest Vibin on GitHub (`dxlbnl/vibin`) and applies
  the changes — adopting seed-owned files the project hasn't touched, reconciling ones it
  has, and following the content-aware steps below for project wiki content. The git diff is
  the source of truth for **what changed**; these migration files explain **why** and carry
  the steps a raw diff can't express.
- Each migration file is named `NNNN-<slug>.md` (a zero-padded, increasing id, purely for
  human ordering) and records the `seed_commit` it shipped at. **The diff decides which run**:
  the migrations to apply are the files that show up as *newly added* in the BASE→LATEST diff —
  no version-number arithmetic.

> Note: migration ids and `docs/proposals/` ids are **independent sequences**. A proposal is
> a design; a migration is a released change. They will usually not line up.

## Migration file format

```markdown
---
id: 1
title: <short title>
status: released            # released | pending
seed_commit: <hash>         # the seed commit that introduced the change, if known
date: <YYYY-MM-DD>
---

## Summary
<one paragraph: what changed and why>

## Detect (is this migration needed?)
<a concrete, read-only check an agent can run to tell whether the project already has this
change — e.g. "does `wiki/architecture.md` contain a `## Rules (binding)` section?">

## Apply — seed-owned tooling
<files under `.claude/**`, `CLAUDE.md`, `README.md` that are NOT project-specific. If a file
is unmodified locally, adopt the latest seed version wholesale; if locally customised, apply
the described edits by hand. List each file + what changed.>

## Apply — project content (content-aware)
<files that hold project-specific content, primarily `wiki/*.md`. NEVER blind-overwrite —
describe how to adapt the project's own content to the new structure.>

## Verify
<checks that confirm the migration landed correctly.>
```

## Rules for writing and running migrations

- **Append-only history.** Respect `wiki/decisions.md`'s rule: a migration never rewrites
  the body of a past decision entry. Header/format-template text is fine to update.
- **One commit per migration run.** `/migrate-vibin` commits the applied changes and writes
  the new seed hash to `.vibin-version`; commit subject `chore: migrate Vibin seed to <short-hash>`.
- **Idempotent detection.** Every migration must carry a `## Detect` check so a re-run, or a
  project that was partially upgraded by hand, is handled safely.
- **Released vs pending.** Only `status: released` migrations are applied by `/migrate-vibin`. A
  `pending` entry documents an upcoming change and is skipped until it ships.
