---
title: Onboarding agents & skills
type: decision
status: draft
---

Skills and agents both have a small **standing** cost (a one-line description in the picker) and a real cost
only **when run** — but they run very differently:

- **Skills run inline** — the body is pulled into the *current* context only when invoked; no cold start. A
  deep, curated library is cheap and welcome. Port the best *mechanisms* (from ECC: intent-driven depth,
  knowledge-ops, deep-research, verification) **rewritten to Vibin's conventions, never bulk-copied**; skip
  domain noise. Each earns its place; one canonical home; overlaps consolidated.
- **Agents run in a cold context** — invoking one spawns a fresh agent that re-derives everything
  ([cost & models](cost-and-models.md)). That cold start, not the description, is the expense. So keep agents
  **few**: add one only for a genuinely recurring, distinct role; keep its description tight; otherwise do the
  work inline with a skill or a `general-purpose` agent.

Admission test for any new agent/skill: does it advance [the loop](the-loop.md)? Is there already an atom/skill
for this ([the atom](the-atom.md) one-home rule)?

**Why:** the cost of an agent is **running it cold**, not having it ([pain P2](../research/vibin-painpoints.md)
— the cold relay was the slowness). A curated *lazy* skill library is an asset; a thicket of cold-spawned agents
is the drag. That is why skills are the primary surface.
