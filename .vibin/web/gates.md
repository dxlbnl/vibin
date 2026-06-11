---
title: Gates (the enforced checkpoints)
type: mechanism
status: draft
---

Gates are *enforced* checkpoints (hooks), not etiquette. The wiki stays honest only if these hold:

- **Read-before-work** — an agent reads the relevant wiki before starting ([retrieval](retrieval.md)). Vibin's
  wiki-gate already does a crude version (read INDEX first).
- **Capture-before-close** — a task is not *done* until its knowledge is captured into the wiki
  ([the loop](the-loop.md) step 3). The keystone gate: it replaces the audit log with a *learning* requirement.
- **Right-size** — heavy ceremony only for risky/irreversible work; otherwise the **test is the spec**
  ([right-sizing](right-sizing.md)).
- **Green-means-green** — no task closes with known-red tests; a persistent failure becomes its own item.

A gate must be **near-free**, or agents route around it (and it slows the loop — [pain P2](../research/vibin-painpoints.md)).

**Why:** the loop only happens if it is enforced, not hoped for. [Pains P6 (no capture) and
P7](../research/vibin-painpoints.md) (red tests rode through 6 "done" items) are both missing-gate failures.
