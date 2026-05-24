---
id: 7
title: interview-me discipline for /bootstrap and /intake
status: released
seed_commit: 845545c
date: 2026-05-24
---

## Summary

`/bootstrap` interviewed in cold batches and `/intake` captured items from a thin title + a
few questions, so both could act on a fuzzy understanding and produce a vague wiki or a thin
card the pipeline then struggled with. This adds a reusable **interview discipline** —
`.claude/skills/interview/SKILL.md` — that drives the artifact-writing entry points to ~95%
confidence before they write: one adaptive question at a time (batch only genuinely
independent ones), infer-and-confirm rather than asking cold, track confidence out loud, a
~6-question cap, a short-circuit the user can take at any point, and a summary-before-acting
step. `/bootstrap` Phase 1 and `/intake` (for vague items) follow it; a clearly-specified ask
still files in one shot. Implements proposal 0004.

## Detect (is this migration needed?)

Needed if **either** is true:

- `.claude/skills/interview/SKILL.md` does **not** exist; or
- `.claude/skills/bootstrap/SKILL.md` Phase 1 does not reference the interview discipline (it
  still says to "ask in batches" without pointing at `.claude/skills/interview/SKILL.md`).

If the interview skill exists and bootstrap/intake reference it, the project is already at this
version — skip.

## Apply — seed-owned tooling

Child-machinery (adopt the latest seed text wholesale if unmodified locally; reconcile by hand
if customized). `git show 845545c -- <path>` shows the exact change when the seed is available.

- `.claude/skills/interview/SKILL.md` — **new**; adopt the seed copy (the discipline + red
  flags + where-used).
- `.claude/skills/bootstrap/SKILL.md` — Phase 1 follows the interview discipline instead of a
  cold batch interview (the topic coverage list is unchanged).
- `.claude/skills/intake/SKILL.md` — Step 2 uses the discipline for vague items; a clear ask
  still files in one shot.

## Apply — project content (content-aware)

None. This migration adds seed tooling; it does not touch project wiki content.

## Verify

Confirm with read-only tools (`Grep`/`Read`/`Glob`) — no Bash, no prompts:

- `.claude/skills/interview/SKILL.md` exists and describes the one-adaptive-question loop with
  a confidence target and a short-circuit.
- `.claude/skills/bootstrap/SKILL.md` Phase 1 references `.claude/skills/interview/SKILL.md`.
- `.claude/skills/intake/SKILL.md` references the interview discipline for vague items and
  keeps the one-shot path for clear asks.
