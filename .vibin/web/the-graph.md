---
title: The knowledge graph
type: principle
status: draft
---

The wiki is a **graph**, not a tree or a list: [atoms](the-atom.md) are nodes, links are edges. There is no
folder hierarchy to obey — an atom links to whatever it relates to. (A `backlog/` dir of transient
[work items](the-backlog.md) is a *lifecycle bucket*, not a navigation hierarchy — you still follow links, not
folders.)

- **One entry point** (the [index](index.md)); from there an agent *navigates* by links, never reads a whole
  directory.
- **One canonical home per fact** — a claim lives in exactly one atom; everything else links to it
  ([what belongs](what-belongs.md)).
- **Density over depth** — a useful atom has several neighbours; an orphan with no links in is invisible
  ([retrieval](retrieval.md)).
- **Links flow one way across kinds** — a [work item](the-backlog.md) links to the knowledge it touches; a
  knowledge atom **never** links to a work item (work items get deleted — a knowledge→work link would dangle).
- **Replace, don't accumulate** — a wrong atom is **deleted** and every inbound link is repointed to its
  replacement (deleting *is* a graph edit: fix the edges). Don't keep dead atoms — that is the graveyard creeping
  back.

**Why:** [pain P6](../research/vibin-painpoints.md) — old Vibin's knowledge pages had *zero* cross-links; it was
a pile of files, not a graph. A graph is navigable by following links; a pile is not.
