# Proposal 0002 — Lite vs full pipeline mode

- **Status:** Implemented — ships as migration `0008` (`migrations/0008-lite-pipeline.md`)
- **Date:** 2026-05-22
- **Affects:** the manager's per-item track dispatch, `/intake`, the backlog item schema
- **Ships as:** migration `0008`

## Context

Every `feature` and `bug` item runs the **full** pipeline: `spec-writer` → `test-writer` →
`implementer` → `reviewer`, with a dedicated spec page and tests-first ceremony. For a
one-line copy fix, a CSS tweak, or a trivial config change that introduces **no new
behavior**, that is disproportionate overhead — a whole spec page and a red→green cycle for
something with nothing meaningful to assert.

Both reference setups recognise this: OpenSpec's guidance is "most changes stay lite;
`design.md` only for risky/cross-cutting work," and Addy's skills reserve heavyweight
planning for non-trivial tasks. Vibin has no such fast path.

## Goal / non-goals

**Goal.** A **lite** track that cuts ceremony for small, low-risk, behavior-neutral
changes, while keeping the full pipeline mandatory for anything that adds or changes
behavior.

**Non-goals.** Weakening tests-first for real behavior; letting "lite" become a bypass.
The qualification gate (below) is the safeguard, and the full suite still runs.

## Design

### The lite qualification gate (what makes an item eligible)

An item MAY take the lite track **only if all** of these hold:

- it touches roughly **≤ a handful of files** and adds **no new dependency**;
- it makes **no schema, API, or public-contract change**;
- it introduces **no new observable behavior that warrants a test** (cosmetic, copy,
  formatting, comment, trivial config, doc-in-code);
- it is **not security-sensitive** (auth, secrets, input handling, permissions).

If **any** condition fails, the item is **full**. When in doubt, full. A `bug` that fixes
real behavior is **always full** (it needs a regression test).

### Selecting the track

- A new optional item-card field/flag, e.g. `mode: lite` (default `full`), set by the user
  at `/intake` time or auto-suggested by the manager when an item clearly qualifies.
- The manager **re-checks the gate** before honoring `lite`; if the item fails the gate it
  silently runs full and notes why in `progress.md`.

### The lite track stages

`implementer` → `reviewer (lite)`:

- **No separate spec page.** The item card's `## Description` plus a one-line acceptance
  check is the contract.
- **Tests-first is conditional**, not skipped by default: if the change has *any* assertable
  behavior, a test is still required (and it falls back to full). Pure-cosmetic changes with
  nothing to assert proceed without a new test.
- **`reviewer (lite)` still runs the full existing suite** (no regressions) and confirms the
  change matches the card + the wiki Rules. It does not check acceptance criteria that don't
  exist.

### Relationship to `chore`

`chore` already skips spec + tests-first, but is scoped to *non-product* work (dep bumps,
doc reorgs, infra). `lite` is for a **small product change** that is too trivial for the
full ceremony but still touches product code. Keep them distinct: `chore` = no product
behavior; `lite` = trivial product behavior.

## Open questions

- `mode: lite` flag vs. a dedicated `type:`? (Leaning flag/modifier on `feature`, so the
  gate and dispatch stay simple.)
- Should the manager auto-promote a lite item to full mid-flight if the implementer
  discovers it's bigger than it looked? (Probably yes — make that an explicit rule.)

## Files the implementation will touch

- `wiki/backlog/README.md` — document the `lite` modifier + the qualification gate.
- `.claude/skills/manager/SKILL.md` — the lite dispatch path, the gate re-check, and the
  auto-promote-to-full rule.
- `.claude/skills/intake/SKILL.md` — let `/intake` suggest/set `lite` for obviously trivial
  asks.
- `.claude/agents/reviewer.md` — a lite review mode (full suite + card/Rules match, no
  acceptance-criteria check).
- `CLAUDE.md` — a one-line rule: lite is gated; behavior changes are full.

## Sources

- OpenSpec workflows ("most changes stay lite") — https://github.com/Fission-AI/OpenSpec
- Addy Osmani `agent-skills` (planning depth scales with risk) —
  https://github.com/addyosmani/agent-skills
