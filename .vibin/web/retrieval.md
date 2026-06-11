---
title: Retrieval (reading the wiki back into work)
type: mechanism
status: accepted
---

Recording knowledge is half the job. **Retrieval is the other half** — the half
[pain P6](../research/vibin-painpoints.md) shows old Vibin missed: the wiki was link-dead *and* unread.

**Implemented shape (proven in the visuals testbed) — three layers:**
1. **Session-start injection** — a SessionStart hook puts the knowledge *map* (the index) + the backlog
   list into every session's context, so the agent starts knowing what exists.
2. **The knowledge gate** ([gates](gates.md)) — writes/Bash/spawns are *blocked* until the actor has read
   the **current** knowledge: only reads under `knowledge/` unlock; **reading a backlog card does not
   count as retrieval**. When the knowledge changes, every actor's marker goes stale and they re-read.
3. **The trigger surface is `tags`** — when an actor reads a work-item card, a hook matches the card's
   words against the atoms' `tags` + titles and surfaces the top relevant atoms as context ("read these
   before working"). The right knowledge is one Read away at the moment work starts.

The gate *forces freshness*; the suggester *points at relevance*; the reviewer checks adherence
([cheap trust](cheap-trust.md)). **Measure reuse** remains the success metric — an atom never retrieved is
dead and gets pruned — but no instrumentation exists yet ([open questions](open-questions.md)).

**Why:** a perfect wiki nobody reads is the old dead wiki with extra steps. Retrieval and reuse — not page
count — are how we know [the loop](the-loop.md) is actually turning.
