---
title: What Vibin is
type: principle
status: draft
---

Vibin is **a knowledge base that builds software** — not a build pipeline that keeps notes.

- **Agents** do the work; several can work against one shared wiki.
- **Knowledge** accumulates as small [atoms](the-atom.md).
- **The wiki** is a [graph](the-graph.md) of those atoms joined by links.
- **The loop** turns work into knowledge and knowledge into better work — see [the loop](the-loop.md).

The knowledge base is the point; code is what agents ship as they work. Identity test: *delete the
pipeline but keep a knowledge base that grows — still Vibin. Keep the pipeline but the knowledge never
grows — not Vibin.* That second case is exactly how `zod4-mock` failed ([pain P6](../research/vibin-painpoints.md)).

**Why:** old Vibin optimised for an audit trail over knowledge, so its wiki became a kanban board
([pains P6/P9/P11](../research/vibin-painpoints.md)). Making the knowledge base the core — and the pipeline
its servant — is the one decision everything else hangs on.
