---
title: Branch-mergeable state
type: decision
status: draft
---

The working state MUST be **mergeable across branches by construction**, so git-flow stops fighting the
workflow ([pain P1](../research/vibin-painpoints.md)).

- **Per-item fragment files**, never a shared append-only `progress.md` / `decisions.md` every task edits at the
  same line → no merge conflicts.
- **Collision-free IDs** (timestamp/slug, not a global `max+1` two branches both grab).
- The wiki is naturally mergeable: separate atoms are separate files; only a genuine edit to the *same* atom
  conflicts ([the knowledge graph](the-graph.md)).
- Worktrees for parallel agents are fine but used "out of true necessity," not by default.
- **A branch carries a [sprint](sprints.md)** — merge it when the sprint **concludes** ([retro](the-retro.md)
  done, learnings captured), not mid-sprint. Parallel work = parallel sprints on their own branches, each merging
  at its own close.

**Why:** P1 — old Vibin's IDs and four monolithic append-files guaranteed a conflict on every parallel item.
Structure the state so branches don't collide, instead of asking humans to avoid it.
