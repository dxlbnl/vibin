---
title: The run log (the sprint narrative)
type: principle
status: draft
---

The run log is the **append-as-you-go narrative** of what is happening *this [sprint](sprints.md)* — the
gotchas, false starts, and fixes as they occur. It lives **inside the sprint file**, below the tasks, and is
the spine the [sprint archive](the-sprint-archive.md) preserves at close.

- **Append-only within a sprint** — add one-liners at meaningful steps; **don't overwrite or trim entries
  mid-sprint**. It is a record you are *building*, not scratch you will delete — and that framing is what makes
  it worth writing. (Called disposable, it got one sparse entry and no more; called a record, it stays rich.)
- **Resumability is a separate pointer.** A short **Next** line just under the tasks says where to pick up; the
  log never has to be scanned or pruned to resume, so append-only costs nothing.
- **The in-flight narrative is homeless otherwise.** A situational gotcha-and-fix is often not durable enough
  for its own [atom](the-atom.md), yet is exactly the trail you'd want when it bites again — `git log` and the
  cards don't hold it. So the log earns its keep, and the [archive](the-sprint-archive.md) **preserves** it
  (enriched with the goal, closed cards, and atom links) rather than reconstructing the sprint from artifacts.
- The single gate: do not close a sprint until its learnings are in the wiki ([gates](gates.md)); the
  sprint-close nudge fires when every task is checked.

**Why:** old Vibin's 1742-line journal + 1290-line decisions log were append-only **forever** — the heaviness
you felt ([pain P3](../research/vibin-painpoints.md)). The cure is a *bounded* log, not *no* log: append-only
**within a sprint**, then archived and cleared from the live file — one bounded file per sprint (like git
history), never the single ever-growing journal. The log is the narrative, the [archive](the-sprint-archive.md)
is the per-sprint ledger, the wiki is the keeper.
