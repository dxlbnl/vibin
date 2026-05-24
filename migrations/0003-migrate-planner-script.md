---
id: 3
title: Single committed planner script for /migrate-vibin (one approval, no ad-hoc python)
status: released
seed_commit: c4878a2
date: 2026-05-24
---

## Summary

`/migrate-vibin` used to tell the agent to improvise the network + diffing with
`curl | python3 -c` one-liners and `for`-loops — so every migration triggered a stream of
per-command permission prompts, and it quietly violated CLAUDE.md's "no ad-hoc `node`/`python`"
rule. The flow now runs through **one committed tool**,
`.claude/skills/migrate-vibin/migrate-plan.py`, allow-listed by its exact invocation. A single
run fetches `BASE..LATEST`, classifies every changed file (child-machinery / seed-meta /
project-content), **auto-applies the child-machinery files that are unchanged locally** (adopting
LATEST is provably safe and git-reversible), and **stages** the rest (customized files'
base+latest, plus the new migration docs) under `.vibin-migrate/` so the agent reconciles and
does the content-aware wiki steps with no further network calls. `--clean` removes the staging
dir via the same allow-listed command.

## Detect (is this migration needed?)

Needed if **either** is true:

- `.claude/skills/migrate-vibin/migrate-plan.py` does **not** exist; or
- `.claude/skills/migrate-vibin/SKILL.md` still instructs the agent to call the GitHub
  **compare** API / `curl` / `mcp__github__*` directly (rather than running `migrate-plan.py`).

If the planner script exists and the skill's Procedure is built around running it, the project
is already at v3 — skip.

> Note: a project adopting this migration still applies it with its **current** migrate flow;
> from the *next* migration onward the single-script flow is in effect.

## Apply — seed-owned tooling

Child-machinery only (adopt wholesale if unmodified locally; reconcile by hand if customized):

- `.claude/skills/migrate-vibin/migrate-plan.py` — **new file**; adopt the seed copy verbatim.
- `.claude/skills/migrate-vibin/SKILL.md` — the Procedure rewritten around the script
  (run planner → apply each staged migration's content-aware steps → reconcile customized
  files from `.vibin-migrate/base|latest` → write `.vibin-version`, commit, `--clean`), plus the
  updated Rules.
- `.claude/settings.json` — **reconcile** (this file is customized per project with a stack
  permission profile): add the one allow-list entry
  `"Bash(python3 .claude/skills/migrate-vibin/migrate-plan.py:*)"` to `permissions.allow`. Do
  **not** broaden it to `Bash(curl:*)` or `Bash(python3:*)` — the point is a single
  narrowly-scoped grant.
- `.gitignore` — add a `.vibin-migrate/` entry (the planner's staging dir). Create the file if
  the project has none.

## Apply — project content (content-aware)

None. This migration changes only seed tooling; it does not touch project wiki content.

## Verify

- `.claude/skills/migrate-vibin/migrate-plan.py` exists and compiles
  (`python3 -m py_compile .claude/skills/migrate-vibin/migrate-plan.py`).
- `python3 .claude/skills/migrate-vibin/migrate-plan.py --clean` runs without a permission
  prompt (the allow-list entry is present in `.claude/settings.json`).
- `.claude/skills/migrate-vibin/SKILL.md`'s Procedure runs the script; no remaining
  instruction to improvise `curl` / `python3 -c` / the compare API by hand.
- `.gitignore` contains `.vibin-migrate/`.
