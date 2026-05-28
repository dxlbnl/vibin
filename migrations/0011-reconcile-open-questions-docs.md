---
id: 11
title: Reconcile open-questions docs to the needs-answers model (post-0005 cleanup)
status: released
seed_commit: 8b64a0f
date: 2026-05-24
---

## Summary

Migration 0005 made a blocking open question bounce the card to `inbox/` with
`flags: [needs-answers]` (the spec-writer no longer re-flags), but several sibling docs still
said "flag `review`" — including `spec-writer.md`'s own anatomy contradicting its updated
"Your task", and `wiki/specs/README.md`'s template-block placeholder contradicting its own
prose. This migration reconciles those docs so the needs-answers/bounce model reads identically
everywhere. It also fills in two index/enumeration gaps that drifted as later migrations
shipped (`needs-answers` missing from the flags lists in `wiki/INDEX.md` and the wiki skill;
`browser-testing` missing from the manager's Practices prose enumeration).

## Detect (is this migration needed?)

Needed if **any** is true:

- `.claude/agents/spec-writer.md` still contains `set \`flags: [review]\`` in its
  Rationalizations row, or "not flagged `review`" in its Red flags;
- `wiki/specs/README.md`'s `## Open questions` format-block placeholder still says
  "flagged `review`";
- `wiki/INDEX.md` or `.claude/skills/wiki/SKILL.md` still says a blocking open question
  "**MUST** flag the item `review`" (the spec-pages convention), or omits `needs-answers`
  from the flags list;
- `.claude/skills/manager/SKILL.md` Practices prose enumerates only 5 core practices (no
  `browser-testing`).

If none of those is true, the project is already at this version — skip.

## Apply — seed-owned tooling

Child-machinery (adopt the latest seed text wholesale if unmodified locally; reconcile by hand
if customized). `git show 8b64a0f -- <path>` shows the exact change when the seed is available.

- `.claude/agents/spec-writer.md` — Rationalizations row + Red flags bullet now reference the
  manager bouncing the item with `needs-answers` (the spec-writer classifies and reports; it
  does **not** re-flag the card).
- `wiki/specs/README.md` — the `## Open questions` template-block placeholder matches the prose
  section "Open questions are answered or deferred" (bounce-to-`inbox/` with `needs-answers`).
- `wiki/INDEX.md` and `.claude/skills/wiki/SKILL.md` — the spec-pages convention uses the
  needs-answers/bounce model, and the flags lists include `needs-answers`.
- `.claude/skills/manager/SKILL.md` — the Practices prose lists all six core practices
  (security, accessibility, debugging, performance, copywriting, browser-testing) so it matches
  its own mapping table.
- `.claude/skills/intake/SKILL.md` — harmonized wording on the `mode` question (`feature`-only;
  a `bug` is always full because it needs a regression test).

Genuine, unrelated `flags: [review]` uses (the risky-item approval checkpoint, the manager's
auto-flag, intake's review-flag question, the backlog schema) are deliberately unchanged.

## Apply — project content (content-aware)

None. This migration changes only seed tooling; it does not touch project wiki content.

## Verify

Confirm with read-only tools (`Grep`/`Read`/`Glob`) — no Bash, no prompts:

- `Grep` for `flag the item \`review\`` / `set \`flags: [review]\`` / `not flagged \`review\``
  in `.claude/**` and `wiki/**` returns **no** match tied to **open questions** (the remaining
  hits are the legitimate review-checkpoint uses).
- `needs-answers` is documented in the flags list of both `wiki/INDEX.md` and
  `.claude/skills/wiki/SKILL.md`.
- `.claude/agents/spec-writer.md`'s Rationalizations + Red flags are internally consistent
  with its "Your task" (no `set flags: [review]` for open questions); `wiki/specs/README.md`'s
  template-block placeholder is consistent with its prose section.
- `.claude/skills/manager/SKILL.md` Practices prose enumerates 6 core practices including
  `browser-testing`.
