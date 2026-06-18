---
title: Gates (the enforced checkpoints)
type: mechanism
status: accepted
---

Gates are *enforced* checkpoints (hooks), not etiquette. The wiki stays honest only if these hold:

- **Read-before-work** — the **knowledge gate**: writes, non-trivial Bash, and agent spawns are blocked
  until the actor has read the *current* knowledge graph ([retrieval](retrieval.md)). Only reads under
  `knowledge/` (or the wiki index) unlock — **reading a backlog card is not retrieval**. Knowledge changes
  invalidate every actor's marker; card/backlog edits do not.
- **Capture-before-close** — the keystone ([capture](capture.md)): a Stop is blocked once while product
  code changed and no atom was written. Replaces the audit log with a *learning* requirement.
- **Sprint-close** — a Stop **nudge** (not a hard block) that fires once when every task in `sprint.md` is
  checked and none are open: run the retro, capture learnings, **archive** the sprint
  ([the archive](the-sprint-archive.md)), then clear the file. Same near-free altitude as capture.
- **Right-size** — heavy ceremony only for risky/irreversible work; otherwise the **test is the spec**
  ([right-sizing](right-sizing.md)).
- **Green-means-green** — no task closes with known-red tests; a persistent failure becomes its own item
  ([verification](verification.md)).

**Knowledge gate: diff on stale, hard block only on missing.** The gate distinguishes two stale
states. *Missing marker* (actor never read the knowledge) → hard block: the actor has no baseline,
work must not proceed. *Stale marker* (knowledge changed since last read) → inject a diff of the
changed atoms as advisory context and exit 0, letting the actor judge relevance. An actor working
on UI who sees a stale `bunq-rate-limits.md` can carry on; one who sees a stale `money-model.md`
knows to re-read. Judgment calls cannot be gated mechanically.

**The gate is worktree-aware.** For [parallel sprints](parallel-sprints.md), each sprint-runner
agent operates inside a git worktree with `CLAUDE_PROJECT_DIR` pointing to the main project. The
gate calls `git worktree list --porcelain` to enumerate all active worktree roots and accepts reads
and writes under any of their `wiki/knowledge/` directories. This means a sprint-runner reading
`.vibin/sprint/014/wiki/knowledge/index.md` satisfies the gate without touching the main project's
copy. The freshness check (newest mtime) also runs per-worktree knowledge dir.

**Subagents start cold.** A spawned subagent (Agent/Task) has a fresh `agent_id` with no gate
marker — its first Write or Bash is hard-blocked even if the parent session's gate is satisfied.
Two fixes: (1) instruct the subagent in its prompt to read `wiki/knowledge/index.md` and relevant
atoms before any other action; (2) do the work inline in the parent session, which already has a
fresh marker. Prefer (2) for tasks that fit in the main context; prefer (1) for agents that
genuinely need isolation (reviewer, researcher, [parallel sprint](parallel-sprints.md) subagents).

A gate must be **near-free**, or agents route around it ([pain P2](../research/vibin-painpoints.md)).
**Scoping is what keeps it free**: the capture gate watches only product-code dirs, so config and card
edits never trigger it — false nudges are how a gate loses its authority.

**Why:** the loop only happens if it is enforced, not hoped for. [Pains P6 and
P7](../research/vibin-painpoints.md) are both missing-gate failures. Knowledge, capture, and sprint-close are
implemented as hooks (the seed); right-size and green-means-green are currently convention in the loop skill + reviewer.

**Ops lesson (learned the hard way):** replace a hook script by **writing over it in place — never
delete-then-recreate**. The harness caches hook config per session; a missing script exits 2 and
hard-blocks *every* gated tool, including the ones you'd use to restore it. When retiring a hook, leave a
no-op stub until the next session restart.
