---
id: 5
title: Resolve spec open questions without stalling the run (bounce to inbox, fold answers back)
status: released
seed_commit: 5368031
date: 2026-05-24
---

## Summary

Migration 0002 gave specs a `## Open questions` gate, but in practice the questions never
reached the user (the manager's review checkpoint only *presented the spec for approval*,
it never asked them) and a user's inline answer got mis-triaged into the backlog instead of
folded into the spec. This migration makes open questions **answered or explicitly deferred —
never silently dropped — without stalling the run**: an item with **blocking** open questions
is bounced back to `wiki/backlog/inbox/` with `flags: [needs-answers]` and the questions
written onto its card, and the manager continues with the next ready item. The user answers on
the card whenever; the item resumes and `spec-writer` folds the answers into the draft spec.
Triage now distinguishes an **answer to an open question** (folds into the current spec) from
genuinely new work (`/intake`).

## Detect (is this migration needed?)

Needed if **either** is true:

- `.claude/skills/manager/SKILL.md` has no open-questions bounce step — i.e. it does not
  mention `needs-answers`; or
- `CLAUDE.md`'s Triage bullet does not distinguish "an answer to an open question" from new
  work filed via `/intake`.

If the manager bounces blocking-question items with `needs-answers` and Triage carves out
answers, the project is already at this version — skip.

## Apply — seed-owned tooling

Child-machinery (adopt the latest seed text wholesale if unmodified locally; reconcile by hand
if customized). `git show 5368031 -- <path>` shows the exact change when the seed is available.

- `.claude/skills/manager/SKILL.md` — the per-item **"Handle open questions"** step (bounce
  blocking-question items to `inbox/` with `flags: [needs-answers]`, continue the run), the
  **"Items awaiting answers"** resume section, the **spec-writer (incorporate answers)**
  delegation template, and the Triage line distinguishing an answer from new work. (The
  pipeline steps were renumbered: gate → 4, review checkpoint → 5, retry → 6, done → 7.)
- `.claude/agents/spec-writer.md` — classify + report open questions but **do not** re-flag
  the card (the manager owns the bounce); the **"Incorporating answers"** section that folds
  user answers into the spec and records deferrals.
- `.claude/skills/intake/SKILL.md` — Rules: an answer to an open question on the active item
  is not new work; do not file it.
- `CLAUDE.md` — Triage bullet: an answer to an open question folds into the current spec,
  never `/intake`.
- `wiki/backlog/README.md` — the `needs-answers` flag + resume convention.
- `wiki/specs/README.md` — the "Open questions are answered or deferred" section.

## Apply — project content (content-aware)

None. This migration changes only seed tooling; it does not touch project wiki content.

## Verify

Confirm with read-only tools (`Grep`/`Read`/`Glob`) — no Bash, no prompts:

- `.claude/skills/manager/SKILL.md` contains a "Handle open questions" step that sets
  `flags: [needs-answers]` and `git mv`s the card to `inbox/` and **continues** (does not stall
  on a synchronous prompt), plus an "Items awaiting answers" resume section and a
  "spec-writer (incorporate answers)" delegation template.
- `.claude/agents/spec-writer.md` has an "Incorporating answers" section and no longer tells
  the spec-writer to set `flags: [review]` itself.
- `CLAUDE.md` Triage and `.claude/skills/intake/SKILL.md` Rules both state that an answer to an
  open question is not filed as new work.
- `wiki/backlog/README.md` documents the `needs-answers` flag; `wiki/specs/README.md` uses the
  answer-or-defer model.
