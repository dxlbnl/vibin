---
title: Bootstrapping the wiki (cold-start a new or v1 project)
type: mechanism
status: draft
---

A project shouldn't need 100 sprints to get a useful wiki. A **bootstrap / migration** workflow ingests what
already exists and distils it into a starter wiki.

- **Sources** — for a fresh project: the codebase, README, configs. For a **Vibin v1** project (e.g.
  `zod4-mock`): the old `wiki/`, `decisions.md`, specs, and Rules — mine them for durable [atoms](the-atom.md),
  drop the audit narration.
- **Process** — read sources → extract candidate facts/decisions/patterns → dedupe to one canonical home →
  write short linked atoms → keep the binding [Rules](the-rules.md) verbatim (they already worked). The output is
  a small, navigable graph, not a dump.
- **Prior art** — the `karpathy-llm-wiki` skill already does exactly this shape (ingest sources → wiki → lint
  quality); port its mechanism rather than reinventing ([onboarding](onboarding-agents-and-skills.md)).
- **bootstrap** (new project) and **migrate-from-v1** are the same ingest-and-distil engine, different sources.

**Why:** without a cold-start, every project begins with an empty wiki and the loop has nothing to
[retrieve](retrieval.md). Bootstrapping seeds the graph so retrieval pays off from task one.
