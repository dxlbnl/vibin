---
title: Knowledge — start here
type: index
status: draft
---

> **Starter template.** `/bootstrap` populates this wiki from the project interview. Until then this
> index is the only page.

Durable, reusable knowledge for this project — short, linked **atoms** (one idea each), grouped by
subject. Read the atoms relevant to your task **before** working (the knowledge gate enforces it);
capture new learnings here when you finish (the capture gate prompts you).

## Groups

Created by `/bootstrap` and grown by work. Typical starting set:

- `project/` — what the project is, **the-rules** (binding constraints, RFC-2119), project structure.
- *(add groups as knowledge accumulates: `testing/`, `architecture/`, domain topics, …)*

## The atom format

```
---
title: <short>
type: principle | mechanism | decision | pattern | reference | question
status: draft | accepted
tags: [<topic>, <topic>]   # the retrieval trigger — matched against work items
---
<2–6 sentences. Link related atoms inline with [label](../group/slug.md).>

**Why:** <the rationale, stated inline; cite sources only as breadcrumbs.>
```

Rules of the graph: **one idea per atom** · **one canonical home per fact** (reconcile, never duplicate)
· link densely (an orphan is invisible) · **replace when wrong** (delete + repoint inbound links) ·
knowledge lives here, **never** in native memory or run logs.
