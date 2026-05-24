# Proposal 0003 — Skill library + skill anatomy

- **Status:** Implemented — Phase 1 → migration `0004`, Phase 2 → migration `0006`
- **Date:** 2026-05-22
- **Affects:** the agent/skill definitions, and a new reusable practice-skill library
- **Ships as:** Phase 1 → migration `0004`; Phase 2 → migration `0006`

## Context

Addy Osmani's `agent-skills` ships two things Vibin lacks:

1. **A library of codified practice skills** — security/OWASP, performance, api-design,
   debugging triage, frontend/a11y, migrations, ADRs — reusable knowledge an agent loads
   when the work calls for it. Vibin has four pipeline roles + ad-hoc specialists, but no
   reusable knowledge base they can pull from.
2. **A hardened skill anatomy** — each skill carries **Rationalizations** (pre-empts the
   agent's excuses with a counter), **Red flags** (symptoms that you're cutting a corner),
   and **Verification** (the concrete evidence required to claim done). Vibin's agent defs
   have good rules but no anti-rationalization defense — exactly the layer that stops the
   shortcuts tests-first is trying to prevent ("this is too small to test", "the test is
   basically the implementation", "I'll refactor later").

## Goal / non-goals

**Goal.** (1) Harden the existing agents against rationalized shortcuts, and (2) give the
pipeline a reusable, loadable body of practice knowledge.

**Non-goals.** Loading every practice into every item (context bloat — load only what's
relevant). Replacing the four pipeline roles; practices are knowledge, not roles.

## Design

### Phase 1 — Skill anatomy on the core agents (cheap, high value)

Add three sections to the agent/skill definitions where shortcuts are most tempting —
primarily `test-writer`, `implementer`, and `reviewer`:

- **Rationalizations → counter.** A short table of the excuse and its rebuttal. E.g.
  test-writer: *"too small to test" → if it has observable behavior, it gets a test;*
  implementer: *"a helper is cleaner" → minimum code first; refactor is a separate item.*
- **Red flags.** Concrete symptoms the agent should catch in itself — e.g. "I changed a
  test to make it pass", "I added a file no criterion asked for", "I asserted a constant the
  test set itself".
- **Verification.** The concrete evidence the agent must produce to claim done — e.g.
  test-writer: *the failing run output and why it fails;* reviewer: *the full-suite summary
  + per-criterion pass.* (Some of this exists; make it an explicit, named section.)

This phase is mostly editing existing prompts and pairs naturally with the `tdd-cycle`
skill (the canonical discipline doc).

### Phase 2 — A practice-skill library

A directory of stack-agnostic practice skills the pipeline can load on demand, e.g.
`.claude/skills/practices/{security,performance,api-design,accessibility,debugging,migrations}/SKILL.md`.
Each follows the same anatomy (knowledge + Rationalizations + Red flags + Verification).

- **Invocation / context-engineering.** The manager (or reviewer) decides which practice
  applies to an item and names it in the delegation prompt — e.g. a security-sensitive item
  loads the security practice; a UI item loads accessibility. Only relevant practices load,
  never the whole library.
- **Seed vs project.** The seed ships a small stack-agnostic core; `/bootstrap` can select
  and tune the practices a given project needs (and add stack-specific ones).

## Open questions

- Where practices live and how they register (`.claude/skills/practices/**` vs a flatter
  layout) so they don't clutter the user-facing slash-command list.
- Who decides which practice loads per item — manager-driven (explicit) vs. agent-pulled
  (the agent recognises it needs it). Leaning manager-driven for auditability.
- How much practice content ships in the seed vs. is added at bootstrap (keep the seed
  lean).

## Files the implementation will touch

- `.claude/agents/{test-writer,implementer,reviewer,spec-writer}.md` — add the anatomy
  sections (Phase 1).
- `.claude/skills/tdd-cycle/SKILL.md` — cross-reference the Rationalizations/Red-flags.
- `.claude/skills/practices/**` — the practice library (Phase 2, new).
- `.claude/skills/manager/SKILL.md` — when/how to load a practice into a delegation prompt.
- `.claude/skills/bootstrap/SKILL.md` — select/tune project-relevant practices.

## Sources

- Addy Osmani `agent-skills` — https://github.com/addyosmani/agent-skills
  (skill anatomy: Rationalizations / Red flags / Verification; the practice-skill set).
