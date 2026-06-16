---
title: Gates (the enforced checkpoints)
type: mechanism
status: accepted
---

Gates are *enforced* checkpoints (hooks), not etiquette. The wiki stays honest only if these hold:

- **Read-before-work** — the **knowledge gate**: writes, non-trivial Bash, and agent spawns are blocked
  until the actor has read the *current* knowledge graph ([retrieval](retrieval.md)). Only reads under
  `knowledge/` (or the wiki index) unlock — **reading a backlog card is not retrieval**. Knowledge changes
  invalidate every actor's marker; card/backlog edits do not.
- **Capture-before-close** — the keystone ([capture](capture.md)): a Stop is blocked once while product
  code changed and no atom was written. Replaces the audit log with a *learning* requirement.
- **Sprint-close** — a Stop **nudge** (not a hard block) that fires once when every task in `sprint.md` is
  checked and none are open: run the retro, capture learnings, **archive** the sprint
  ([the archive](the-sprint-archive.md)), then clear the file. Same near-free altitude as capture.
- **Right-size** — heavy ceremony only for risky/irreversible work; otherwise the **test is the spec**
  ([right-sizing](right-sizing.md)).
- **Green-means-green** — no task closes with known-red tests; a persistent failure becomes its own item
  ([verification](verification.md)).

A gate must be **near-free**, or agents route around it ([pain P2](../research/vibin-painpoints.md)).
**Scoping is what keeps it free**: the capture gate watches only product-code dirs, so config and card
edits never trigger it — false nudges are how a gate loses its authority.

**Why:** the loop only happens if it is enforced, not hoped for. [Pains P6 and
P7](../research/vibin-painpoints.md) are both missing-gate failures. Knowledge, capture, and sprint-close are
implemented as hooks (the seed); right-size and green-means-green are currently convention in the loop skill + reviewer.

**Ops lesson (learned the hard way):** replace a hook script by **writing over it in place — never
delete-then-recreate**. The harness caches hook config per session; a missing script exits 2 and
hard-blocks *every* gated tool, including the ones you'd use to restore it. When retiring a hook, leave a
no-op stub until the next session restart.
