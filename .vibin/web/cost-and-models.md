---
title: Cost & models (spend where it pays)
type: principle
status: draft
---

A grand setup is **cost-aware**: the cheapest move that does the job.

- **Model selection** — a strong default model for most work; escalate to the most capable only when a first
  attempt failed, the change spans many files, or it is architectural / security-critical.
- **Inline over spawn** — a skill runs *inline* in the current context; spawning an
  [agent](onboarding-agents-and-skills.md) starts a **cold** context that re-derives everything. Prefer inline;
  spawn only when a task is genuinely separable. When you do spawn, pass the **objective ("why")**, not just the
  task, and allow a few (≤3) follow-ups before accepting the result.
- **Point, don't copy** — give an agent **links** to the relevant [atoms](the-atom.md) and let it
  [retrieve](retrieval.md) them; never paste wiki content into the prompt (that duplicates a fact's one
  canonical home — [what belongs](what-belongs.md)).

**Why:** old Vibin's cold 4-agent relay paid full context tax on every item
([pain P2](../research/vibin-painpoints.md)). Cheapness is a feature: inline beats cold-spawn, links beat copies,
the default model beats the dear one.
