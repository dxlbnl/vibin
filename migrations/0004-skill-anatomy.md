---
id: 4
title: Skill anatomy on the pipeline agents (Rationalizations / Red flags / Verification)
status: released
seed_commit: 4b54f0d
date: 2026-05-24
---

## Summary

Vibin's pipeline agents had good rules but **no anti-rationalization defense** — the layer
that stops the shortcuts tests-first is meant to prevent ("too small to test", "the test
mirrors the implementation", "I'll refactor later", "green is enough, pass it"). Each pipeline
agent now carries a consistent **skill anatomy**: a `## Rationalizations → rebuttals` table, a
`## Red flags` self-check list, and a `## Verification` (the concrete evidence required to
claim done). The full trio is on `test-writer`, `implementer`, and `reviewer`; a lighter set
(Rationalizations + Red flags) is on `spec-writer`. `tdd-cycle` cross-references the anatomy
so it is documented once. This is **Phase 1** of proposal 0003; Phase 2 (the practice-skill
library) is deferred — see `docs/proposals/README.md`.

## Detect (is this migration needed?)

Needed if the pipeline agents lack the anatomy — concretely, if
`.claude/agents/test-writer.md` (or `implementer.md` / `reviewer.md`) has **no**
`## Red flags` section (equivalently, no `## Rationalizations → rebuttals`).

If all three core agents already have `## Rationalizations → rebuttals`, `## Red flags`, and
`## Verification`, the project is already at this version — skip.

## Apply — seed-owned tooling

Child-machinery (adopt the latest seed text wholesale if unmodified locally; reconcile by
hand if customized). `git show 4b54f0d -- <path>` shows the exact change when the seed is
available.

- `.claude/agents/test-writer.md` — add `## Rationalizations → rebuttals`, `## Red flags`,
  and `## Verification (evidence to claim done)`; the trailing "Report back" Rules bullet now
  points at the Verification section.
- `.claude/agents/implementer.md` — same trio; "Report back" Rules bullet points at
  Verification.
- `.claude/agents/reviewer.md` — same trio (`## Verification (evidence to claim PASS)`),
  appended after the existing "Your report" section.
- `.claude/agents/spec-writer.md` — the lighter set: `## Rationalizations → rebuttals` and
  `## Red flags` (no separate Verification — its report-back already covers the evidence).
- `.claude/skills/tdd-cycle/SKILL.md` — a short `## Anti-rationalization anatomy` section
  pointing at the four agents' anatomy sections.

These sections **reference** the existing boxes (test-writer's "trivially-passing",
implementer's "minimum", the RFC-2119 requirement/scenario rules) rather than restating them
— preserve that when reconciling a customized agent.

## Apply — project content (content-aware)

None. This migration changes only seed tooling; it does not touch project wiki content.

## Verify

Confirm with read-only tools (`Grep`/`Read`/`Glob`) — no Bash, no prompts:

- `.claude/agents/test-writer.md`, `implementer.md`, and `reviewer.md` each contain all three
  headers: `## Rationalizations → rebuttals`, `## Red flags`, and a `## Verification …`.
- `.claude/agents/spec-writer.md` contains `## Rationalizations → rebuttals` and `## Red flags`.
- `.claude/skills/tdd-cycle/SKILL.md` contains `## Anti-rationalization anatomy` referencing
  the agent files.
