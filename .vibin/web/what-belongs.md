---
title: What belongs in the wiki (and what must not)
type: principle
status: draft
---

**In** — durable knowledge only:
- Decisions and their *why* (the binding [Rules](the-rules.md) are the load-bearing ones).
- Reusable patterns (Vibin knowledge, see [the atom](the-atom.md)).
- Project facts a future task would otherwise re-derive.

**Out** — keep it out:
- Audit-trail narration ("agent did X then Y") — that is run-log scratch, disposable ([the run log](the-run-log.md)).
- Anything restating what a test already asserts — precision lives in tests; prose carries intent.
- A second copy of an existing atom — reconcile, don't duplicate ([the atom](the-atom.md), one home).
- Per-task ceremony kept forever — the old kanban graveyard ([pains P6/P9/P11](../research/vibin-painpoints.md)).

*Work items* (the [backlog](the-backlog.md)) also live in the graph but are **transient** scaffolding, deleted
when done — this page governs the durable knowledge nodes.

**Wiki only, never native memory.** All knowledge lives in wiki atoms — visible, in-repo, reviewable. Claude's
native `~/.claude/.../memory/` is **opaque** and per-machine; do not use it for project knowledge.

**Budgets** (leanness is measured, see [onboarding agents & skills](onboarding-agents-and-skills.md)): an atom is
short; the *always-on* footprint (agent descriptions, enabled MCP tools, `CLAUDE.md`) stays small; the skill
library is lazy.

**Why:** the wiki is useful only if it is *small and true*. Completeness is not a goal; usefulness is — enforced
by [retrieval](retrieval.md) (unread atoms get pruned).
