---
title: Cheap trust (verify intent in seconds)
type: principle
status: draft
---

Removing the audit trail removed the human's way to *check* the work. Replace it with **cheap trust**, not
nothing: a human should confirm the right thing was built in seconds, without reading everything.

- Each task leaves a **one-paragraph "what changed and why"** — cheaper than a spec, richer than a bare diff.
- Trust rests on three glanceable things: the **acceptance criteria**, the **test diff**, and the **wiki diff**
  (what knowledge this task added).
- Watch the trap: green tests an agent wrote itself can pass while *missing the point* — so trust checks
  **intent satisfied**, not only [tests green](verification.md).

**Why:** [pains P3/P4 vs P7](../research/vibin-painpoints.md) — the 587-line spec was unreadable *and* the suite
still shipped red. Leanness is only safe if trust stays cheap; otherwise we just hid the risk.
