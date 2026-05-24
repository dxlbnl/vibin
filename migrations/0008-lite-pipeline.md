---
id: 8
title: Gated lite pipeline track (mode: lite) for trivial behavior-neutral changes
status: released
seed_commit: f56accf
date: 2026-05-24
---

## Summary

Every `feature`/`bug` ran the full pipeline (`spec-writer` → `test-writer` → `implementer` →
`reviewer`), which is overkill for a one-line copy fix, a CSS tweak, or a trivial config change
that introduces no new behavior. This adds a **lite** track: a `feature`/`bug` card may carry
**`mode: lite`** to run `implementer` → `reviewer (lite)` with no spec page and no tests-first.
Lite is **gated** (small, behavior-neutral, no schema/API/security) and the manager
**auto-promotes to full** if a change turns out bigger — so lite is never a tests-first bypass.
Implements proposal 0002.

## Detect (is this migration needed?)

Needed if **either** is true:

- `wiki/backlog/README.md` documents no `mode: lite` / lite gate; or
- `.claude/skills/manager/SKILL.md` has no lite dispatch (no `mode: lite` handling — grep
  `mode: lite`).

If the backlog schema documents `mode: lite` + the gate and the manager dispatches lite items
to `implementer` → `reviewer (lite)`, the project is already at this version — skip.

## Apply — seed-owned tooling

Child-machinery (adopt the latest seed text wholesale if unmodified locally; reconcile by hand
if customized). `git show f56accf -- <path>` shows the exact change when the seed is available.

- `wiki/backlog/README.md` — the `mode:` card field, the **Lite mode** section (the 4-condition
  gate + the `chore` vs `lite` distinction), and the Tracks-table note.
- `.claude/skills/manager/SKILL.md` — the **Lite track** section (gate, dispatch, gate
  re-check → silent downgrade-to-full, mid-flight auto-promote to full) and the two delegation
  templates `implementer (lite)` and `reviewer (lite)`.
- `.claude/agents/reviewer.md` — the **Lite review** mode (match-to-card + full-suite-green +
  behavior-neutral; FAIL "needs the full track" on a mis-classified item).
- `.claude/skills/intake/SKILL.md` — Step 2 can suggest/set `mode: lite` for an obviously
  trivial behavior-neutral ask (the manager re-checks the gate); the card template shows the
  optional `mode:` line.
- `CLAUDE.md` — the Workflow lite bullet, the **Lite track** operational rule, and the
  tests-first line scoped to **full** items.

## Apply — project content (content-aware)

None. This migration adds seed tooling; it does not touch project wiki content. Existing item
cards default to full (no `mode:` field) — no change needed.

## Verify

Confirm with read-only tools (`Grep`/`Read`/`Glob`) — no Bash, no prompts:

- `wiki/backlog/README.md` documents the `mode:` field and the 4-condition lite gate, and
  distinguishes `chore` from `lite`.
- `.claude/skills/manager/SKILL.md` dispatches `mode: lite` to `implementer (lite)` →
  `reviewer (lite)`, re-checks the gate before honoring lite, and auto-promotes to full when a
  change exceeds the gate.
- `.claude/agents/reviewer.md` has a "Lite review" section that FAILs a mis-classified lite
  item with "needs the full track".
- `.claude/skills/intake/SKILL.md` can set `mode: lite`; `CLAUDE.md` carries the lite rule and
  scopes tests-first to full items.
