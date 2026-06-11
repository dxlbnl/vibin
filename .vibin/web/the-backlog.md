---
title: Work items & the backlog
type: mechanism
status: draft
---

Work lives in the graph too — as **work-item nodes**, the transient sibling of durable
[knowledge atoms](the-atom.md). They sit in a `backlog/` dir (a lifecycle bucket, still navigated by links —
[the graph](the-graph.md)) and carry **proportional detail**: a line for a trivial idea; a repro, fault
mechanism, and fix approach for a real bug. A single line was too austere — capture what the work needs.

- **Lifecycle** — a work item flows: **backlog** (not started) → **[sprint](sprints.md)** (active) → **done**.
  On done, its durable learnings are extracted to knowledge atoms ([capture](capture.md)) and the work item is
  **deleted** ([replace, don't accumulate](the-graph.md)). The graveyard was *retaining* done cards forever
  ([pains P6/P9](../research/vibin-painpoints.md)) — not detail.
- **The card is the spec** — when a work item needs speccing (the heavy path, [right-sizing](right-sizing.md)),
  the detail is written *into the work item itself* — never a separate spec file. One home, always current; old
  Vibin's card + separate spec page was [pain P4](../research/vibin-painpoints.md) (four copies of intent).
- **Hierarchy, if need be** — decompose only when work is big: **epic → feature → story**, as linked items
  (parent links children). Most work is a single story; don't force the ladder.
- **Links go one way** — a work item links to the knowledge it touches and to its siblings/parent; a knowledge
  atom **never** links back to a (deletable) work item ([the graph](the-graph.md)). So work reuses what's known
  ([retrieval](retrieval.md)) without leaving dangling links when it's deleted.
- **Named by slug, not number** — cards are `backlog/<slug>.md`. Slug filenames can't collide across
  branches the way a global `B<max+1>` counter did ([branch-mergeable state](branch-mergeable-state.md));
  a slug collision means the same topic — update that card (one home).
- **Cards link knowledge, never restate it** — a card points at the atoms it builds on (work → knowledge);
  a card that restates an *overruled* design actively misleads (the visuals sweep found five cards still
  instructing agents to build a rejected `Param` layer — keep cards current when decisions change).
- **Intake** — items come from your ideas, **GitHub issues** (a `gh-issue-intake` skill), and
  [retro](the-retro.md) proposals. A GitHub item is closed by a `fixes #N` commit, then deleted.

**Why:** you were right — a one-liner can't hold a fault mechanism or a fix. Work items carry real detail (their
own spec) and live in the graph; leanness comes from **deleting them when done** and keeping the durable output
as knowledge, not from starving the description.
