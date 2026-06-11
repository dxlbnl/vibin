---
title: The atom (a unit of knowledge)
type: decision
status: draft
---

One atom = one idea, short, linked. This page is itself an atom, in the format atoms use:

```
---
title: <short>
type: principle | mechanism | decision | question
status: draft | accepted | superseded
---
<2–6 sentences. Link related atoms inline with [label](other-atom.md).>

**Why:** <the rationale / evidence, linking sources.>
```

An atom is: short enough to read in one breath; has **one** canonical home (never a second copy); linked to
its neighbours; superseded by `status:` + a link, never edited into a contradiction.

## Two kinds of knowledge
- **Project knowledge** — facts/decisions for one product (e.g. "we use pnpm"). *True until superseded*;
  **no confidence score** (a fact is not a probability). Lives in that project's wiki.
- **Vibin knowledge** — cross-project working knowledge ("when X breaks, check Y first"). *Confidence-
  weighted*, earned by recurrence across projects. Lives in Vibin's own wiki — where a confidence score
  actually pays off.

**Why:** [pains P3/P4](../research/vibin-painpoints.md) — the 587-line spec was the opposite of an atom.
Separating the two kinds avoids scoring project facts with confidence numbers nobody updates.
