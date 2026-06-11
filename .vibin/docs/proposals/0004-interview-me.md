# Proposal 0004 — interview-me discipline for bootstrap & intake

- **Status:** Proposed (not yet implemented)
- **Date:** 2026-05-22
- **Affects:** `/bootstrap` (the interview phase) and `/intake`
- **Ships as:** a future `migrations/` entry once implemented

## Context

`/bootstrap` interviews the user in batches, and `/intake` captures a new item from a short
title + a few questions. Both can act on a fuzzy understanding, producing a vague wiki or a
thin item card that the pipeline then struggles with. Addy's **interview-me / idea-refine**
pattern instead asks **one focused question at a time**, adapting each question to the last
answer, and only proceeds once it is ~95% confident it can produce a good artifact. The
result is sharper specs and fewer wrong turns — the cost of a question is far lower than the
cost of building the wrong thing.

## Goal / non-goals

**Goal.** A reusable interviewing discipline that drives `/bootstrap` and `/intake` to high
confidence before they write the wiki / the item card.

**Non-goals.** Interrogating the user. The discipline must stay fast: batch genuinely
independent questions, and always let the user short-circuit ("good enough, go").

## Design

### The discipline

A documented, reusable pattern (a short skill or a conventions section both entry points
reference):

1. **One adaptive question at a time** (via `AskUserQuestion`) when the next question
   depends on the previous answer; **batch** only questions that are truly independent.
2. **Track confidence.** After each answer, restate the current understanding briefly and
   ask the single highest-value remaining question. Continue until ~95% confident the
   artifact (bootstrap wiki / item card) can be written well.
3. **Infer and confirm, don't ask cold.** Propose a sensible default and ask the user to
   confirm/correct, rather than asking open-ended.
4. **Short-circuit always available.** The user can say "that's enough, proceed" at any
   point; the agent then records remaining unknowns as **Open questions** (for bootstrap) or
   in the item card's `## Notes` (for intake) rather than blocking.
5. **Summarize before acting.** Present the final understanding for a yes before writing.

### Where it applies

- **`/bootstrap` Phase 1** — replace the batch interview with the adaptive discipline,
  still covering project / stack / package-manager / testing / constraints / initial
  backlog, but converging question-by-question.
- **`/intake`** — when a reported item is vague, ask the one or two questions that make it
  actionable (type, the core behavior, priority, whether it's `review`), then file it.
  A clearly-specified ask still files in one shot — don't add friction where there's none.

## Open questions

- Implement as a standalone `.claude/skills/interview/SKILL.md` that bootstrap/intake invoke,
  or as a shared conventions section both reference? (Leaning a small shared skill, so the
  ~95%-confidence loop lives in one place.)
- A hard cap on questions before forcing a summary, to guarantee it never stalls.

## Files the implementation will touch

- `.claude/skills/bootstrap/SKILL.md` — Phase 1 adopts the adaptive interview.
- `.claude/skills/intake/SKILL.md` — adaptive questioning for vague items; one-shot for
  clear ones.
- (possibly) `.claude/skills/interview/SKILL.md` — the shared discipline, new.

## Sources

- Addy Osmani `agent-skills` — https://github.com/addyosmani/agent-skills
  (`interview-me` / idea-refine: one question at a time to ~95% confidence).
