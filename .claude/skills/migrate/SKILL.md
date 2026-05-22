---
name: migrate
description: Bring an existing Vibin project up to date with the latest seed by applying pending migrations from migrations/. Use when the user says "migrate", "upgrade Vibin", "apply the latest seed changes", or after pulling a newer seed. Detects the project's version from .vibin-version and applies each newer migration in order.
disable-model-invocation: false
---

# Migrate a Vibin project to the latest seed

Vibin is cloned per project, so seed improvements do not arrive automatically. This skill
applies the **pending migrations** in `migrations/` to bring the project up to date. Each
migration is a structured, executable upgrade (see `migrations/README.md`).

## STEP 0 — read the wiki (mandatory, enforced)

**Read `wiki/INDEX.md` first.** A `PreToolUse` hook blocks writes and Bash until you do.
The migrations touch the wiki, so you must be working from the current source of truth.

## Procedure

1. **Determine the current version.** Read `.vibin-version` at the repo root — a single
   integer. If the file does not exist, treat the project as version **0**.
2. **Find pending migrations.** List `migrations/NNNN-*.md`. Select those whose `id` (from
   frontmatter) is **greater than** the current version **and** whose `status:` is
   `released`. Sort ascending by id. If none, report "already up to date (vN)" and stop.
3. **For each pending migration, in order:**
   a. Read the migration file.
   b. Run its **`## Detect`** check (read-only). If the change is already present, skip this
      migration (note it) and move on — migrations are idempotent by design.
   c. Apply **`## Apply — seed-owned tooling`**: for each listed file under `.claude/**`,
      `CLAUDE.md`, `README.md`, adopt the new seed text. If the file is unmodified from the
      seed, replace it wholesale; if it was customised locally, apply the described edits by
      hand without discarding the customisation.
   d. Apply **`## Apply — project content (content-aware)`**: follow the steps for
      `wiki/*.md` and other project-specific files. **Never blind-overwrite** project
      content — adapt the project's own content to the new structure. **Never rewrite the
      body of a past `wiki/decisions.md` entry** (append-only).
   e. Run the migration's **`## Verify`** block and confirm it passes. If it does not, stop
      and report — do not proceed to later migrations on a broken state.
4. **Record the new version.** Write the highest applied id to `.vibin-version`.
5. **Commit.** Stage the changed files and `.vibin-version` and commit with subject
   `chore: migrate Vibin seed to v<n>`. Do not push unless the user asks.
6. **Report**: which migrations were applied, which were skipped (already present), the new
   `.vibin-version`, and anything that needs the user's attention.

## Rules

- **One concern per run.** Apply migrations strictly in id order; never skip ahead past a
  failing one.
- **Read-only first.** Always run `## Detect` before changing anything, so a partially
  upgraded or already-current project is handled safely.
- **Respect `decisions.md` append-only** — header/format-template text may be updated, past
  entry bodies may not.
- **No ad-hoc interpreters.** Apply edits with `Read`/`Edit`/`Write`; use `git mv` for moves;
  do not write throwaway `node`/`python` scripts to transform files (see CLAUDE.md).
- **This seed itself is already at the latest version**, so running `/migrate` here reports
  "already up to date". The skill is for downstream clones.
