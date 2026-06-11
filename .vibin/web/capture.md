---
title: Capture (recording knowledge automatically)
type: mechanism
status: accepted
---

The keystone of [the loop](the-loop.md): a task must **record what it learned without anyone choosing to
write it down**. If capture depends on goodwill it doesn't happen — that is exactly
[pain P6](../research/vibin-painpoints.md).

**Implemented shape (proven in the visuals testbed):** a deterministic hook pair.
- *PostToolUse* tracks a per-actor **dirty flag**: set when **product code** changes (scoped to code dirs
  like `src/` — config, card, and wiki edits never count, so no false nudges); cleared when a knowledge
  atom is written.
- *Stop / SubagentStop* blocks **once** while dirty: "capture any durable, reusable learning as an atom —
  **update the existing atom if one covers the topic** (one home) — or say in one line that nothing came
  up." Loop-protected (`stop_hook_active` + clear-on-nudge: one nudge per batch of edits, never a loop).
- The hook guarantees the *moment*; the agent does the *distilling*. First live run: a tap-tempo fix was
  captured into the existing clock atom — updated in place, not duplicated.

**Known risk:** nudge-per-stop can fatigue on long multi-turn tasks (reflex "nothing durable" answers).
Watch for it; scoping to product code is the first mitigation.

**Why:** capture was the highest-risk assumption in [what Vibin is](what-vibin-is.md); a hook, not
goodwill, is what makes the wiki grow. See [retrieval](retrieval.md) for the other half.
