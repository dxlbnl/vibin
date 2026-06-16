---
title: The run log (working scratch)
type: principle
status: draft
---

The run log is the **disposable** tier: a live, append-as-you-go record of what is happening *this
[sprint](sprints.md)* — for resumability and orientation, not for posterity. It lives **inside the sprint file**,
not in a separate growing journal.

- It MAY be freely compressed, trimmed, or **discarded** once its knowledge is captured into the wiki.
- There is **no** requirement to log faithfully as you go — so **don't depend on it for the audit trail**.
  The [sprint archive](the-sprint-archive.md) (the write-once ledger) is **composed at close** from real
  artifacts — closed cards, captured atoms, and `git log` — *not* transcribed from this log. That keeps the
  log near-free ([gates](gates.md)) while the archive stays complete.
- The single gate: do not close a sprint until its learnings are in the wiki ([gates](gates.md)); the
  sprint-close nudge fires when every task is checked.

**Why:** old Vibin's 1742-line journal + 1290-line decisions log were append-only forever — the heaviness you
felt ([pain P3](../research/vibin-painpoints.md)). The cure isn't *no* ledger but a *bounded* one: the log is
scratch, the [archive](the-sprint-archive.md) is the one-file-per-sprint ledger, the wiki is the keeper. (Real
use showed the as-you-go log gets one sparse entry and no more — so the archive is reconstructed at close, not
relied on mid-sprint.)
