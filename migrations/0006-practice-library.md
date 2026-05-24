---
id: 6
title: Practice-skill library loaded on demand (proposal 0003 Phase 2)
status: released
seed_commit: c156150
date: 2026-05-24
---

## Summary

Completes proposal 0003. Phase 1 (migration 0004) hardened the agents with the
Rationalizations / Red-flags / Verification anatomy; this **Phase 2** adds a reusable body of
**practice knowledge** the pipeline loads on demand. Practices live as plain reference docs in
`.claude/practices/<name>.md` — deliberately **not** registered skills, so they don't clutter
the slash-command list — each carrying the same anatomy (Knowledge / Rationalizations / Red
flags / Verification). Loading is **manager-driven**: the manager picks the practice(s) an item
needs (from its `type:`, spec, and risk) and names the file(s) in the delegation prompt; the
reviewer loads the same one(s). The seed ships a lean, stack-agnostic core —
**security, accessibility, debugging, performance, copywriting** — and `/bootstrap` tunes the
set per project and adds stack/domain-specific practices.

## Detect (is this migration needed?)

Needed if `.claude/practices/` does **not** exist (or has no `README.md`), or
`.claude/skills/manager/SKILL.md` has no `## Practices` section.

If `.claude/practices/` holds the core docs and the manager has a `## Practices` section that
names practices in delegation prompts, the project is already at this version — skip.

## Apply — seed-owned tooling

Child-machinery (adopt the latest seed text wholesale if unmodified locally; reconcile by hand
if customized). `git show c156150 -- <path>` shows the exact change when the seed is available.

- `.claude/practices/README.md` and `.claude/practices/{security,accessibility,debugging,
  performance,copywriting}.md` — **new**; adopt the seed copies. (A project that already tuned
  this set keeps its tuning; only add the ones it lacks.)
- `.claude/skills/manager/SKILL.md` — the `## Practices` section (manager-driven selection +
  mapping) and the delegation-template note to add a `Practices: read and apply
  .claude/practices/<name>.md` line where one applies.
- `.claude/skills/bootstrap/SKILL.md` — the Phase-3 step to tune the practice set and add
  project/stack-specific practices.

## Apply — project content (content-aware)

None. This migration adds seed tooling; it does not touch project wiki content. A project may
**prune** practices it doesn't need and **add** its own afterwards (that is expected, not a
migration concern).

## Verify

Confirm with read-only tools (`Grep`/`Read`/`Glob`) — no Bash, no prompts:

- `.claude/practices/` contains `README.md` plus `security.md`, `accessibility.md`,
  `debugging.md`, `performance.md`, `copywriting.md`.
- Each practice doc has `## Knowledge`, `## Rationalizations → rebuttals`, `## Red flags`, and
  `## Verification`.
- `.claude/skills/manager/SKILL.md` has a `## Practices` section describing manager-driven,
  load-only-relevant selection, and the delegation templates mention a `Practices:` line.
- `.claude/skills/bootstrap/SKILL.md` tells bootstrap to tune `.claude/practices/`.
- Practices are **not** registered as skills (no `.claude/skills/practices/**`).
