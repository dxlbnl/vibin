---
title: Cost & models (spend where it pays)
type: principle
status: draft
---

A grand setup is **cost-aware**: the cheapest move that does the job.

- **Model selection** — a strong default model for most work; escalate to the most capable only when a first
  attempt failed, the change spans many files, or it is architectural / security-critical.
- **Spawn vs inline** — a fresh agent starts cold and re-derives context; that is the expensive path. Prefer
  inline work; [spawn an agent](onboarding-agents-and-skills.md) only when a task is genuinely separable.
- **Context is budget** — the always-on footprint and re-reads are real cost ([what belongs](what-belongs.md));
  pass a focused context pack, not the whole wiki.

**Why:** absent from old Vibin entirely — its cold 4-agent relay paid full context tax on every item
([pain P2](../research/vibin-painpoints.md)). Cheapness is a feature, not an afterthought.
