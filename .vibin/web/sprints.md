---
title: Sprints (the rolling working state)
type: mechanism
status: accepted
---

Work is organised as **rolling sprints**, not an ever-growing spec'd backlog. The working state is the *current*
sprint — a short task list (pulled from [the backlog](the-backlog.md)) plus a live [run log](the-run-log.md),
kept in **one file** (e.g. `sprint.md` in the project) — and nothing else.

- Only the current sprint is *detailed*; a closed sprint is **archived** to one bounded, write-once
  file ([the sprint archive](the-sprint-archive.md)), and its live scratch is cleared.
- A sprint **closes** with a [retro](the-retro.md), and only once its durable learnings are captured
  ([gates](gates.md) capture-before-close). Then it is archived and the live file is cleared.
- The backlog stays small and current, not a 120-card archive ([pains P6/P8/P9](../research/vibin-painpoints.md)).

**Why:** the **three-tier rule** — the **wiki keeps the *learnings*** (timeless), [the archive](the-sprint-archive.md)
**keeps the *ledger*** (what shipped, when — write-once per sprint), and **the sprint file is the *live state*** (cleared at close). The
learnings that must outlive a sprint reach the wiki ([what belongs](what-belongs.md)); the chronology the wiki
deliberately omits is the archive's job. (Earlier this was a two-tier rule with finished sprints simply *gone*;
the archive was added back as a bounded ledger — write-once, never the append-forever journal of [pain P3](the-run-log.md).)
See [the work loop](the-work-loop.md) for how tasks in a sprint are run. Implemented as `wiki/sprint.md`: resume
from its **Next** pointer; append to the run log as you go; retro → archive (preserve the log, enrich with
goal/cards/atoms) → clear.
