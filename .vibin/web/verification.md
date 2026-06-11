---
title: Verification (green, and the right green)
type: mechanism
status: draft
---

Verification is how an agent *and* the human know the work is right.

- **Green-means-green** — a task does not close with known-red tests; a persistent failure becomes its own item
  ([gates](gates.md)) ([pain P7](../research/vibin-painpoints.md)).
- **The test is the spec** — precision lives in executable tests, not restated in prose
  ([right-sizing](right-sizing.md)).
- **Reliability when it matters** — pass@k (one of k attempts works) for "just make it work"; pass^k (all k
  consistent) for must-not-regress paths. Optional, for critical surfaces.

**Why:** P7 — six "done" items shipped red because "full suite green" quietly eroded. Verification must be
enforced and, where it matters, expressed as reliability — paired with [cheap trust](cheap-trust.md) so the
human can confirm *intent*, not just green.
