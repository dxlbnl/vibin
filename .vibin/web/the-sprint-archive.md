---
title: The sprint archive (the write-once ledger)
type: mechanism
status: draft
---

The **third tier** of the working state: a bounded, **write-once** record of each closed
[sprint](sprints.md) — one file per sprint at `wiki/sprint-archive/NNNN-slug.md`. It answers *what
shipped, and when*, the chronology the wiki deliberately omits. Distinct from the two other tiers:
the [wiki](the-graph.md) keeps timeless **learnings**, the [run log](the-run-log.md) is the live
append-only **narrative**, and the archive is the durable **ledger**.

- **The run log is the spine; enrich, don't reconstruct.** Built once during the [retro](the-retro.md)
  by **preserving the sprint's append-only [run log](the-run-log.md)** (the in-flight narrative) and
  enriching it with the goal, the cards that closed, and links to the atoms captured. Reconstructing
  from `git log` + cards alone would drop the situational gotcha-and-fix tier only the log carries.
- **Write-once and bounded.** One file per sprint, never appended to after close. That is what keeps
  it from becoming old Vibin's append-forever journal ([pain P3](../research/vibin-painpoints.md)) — a
  ledger of bounded files, not one growing log.
- **Not knowledge.** A durable *learning* still becomes an [atom](the-atom.md); the archive records
  *history*, and history is not retrieved into work. Don't link work → archive; link work → wiki.
- **Enforcement.** The sprint-close [gate](gates.md) nudges the archive-and-clear ceremony when every
  task in `wiki/sprint.md` is checked.

**Why:** the wiki's strength — timeless, deduplicated knowledge — is also a gap: it can't tell you the
order things happened or what a given sprint actually delivered. An audit trail and a reference of past
sprints are legitimately durable, but only if bounded; the write-once-per-sprint file gives the trail
back without the heaviness [pain P3](../research/vibin-painpoints.md) warned against.
