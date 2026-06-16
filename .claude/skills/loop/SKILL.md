---
name: loop
description: Run the Vibin v2 build loop — pull a work item into the sprint, then retrieve → do → verify → capture → review → close, mostly inline. Use to build the backlog or resume work.
---

# The loop — Vibin v2 build cycle

You (the working session) run a work item end to end, **mostly inline**. There is no spec→test→implement
relay: the card is the spec, tests are the spec, and you do the work yourself. Only **review** and
**research** are separate (cold) agents.

## Working state — the sprint
`wiki/sprint.md` is the one working-state file: the sprint's task list + a disposable run log.
- **Resuming**: read `wiki/sprint.md` first — it says what's in flight. Tasks remain → continue them.
  No sprint file or all tasks done → run **Sprint start** (below) before touching any card.
- **Log as you go**: the run log is **append-only** — add one-liners at meaningful steps; never
  overwrite or trim entries mid-sprint (it's the narrative the archive preserves). Put a short
  **Next** pointer just below the Tasks for resumability — not in the log. Knowledge does NOT live here.

## Sprint start — plan, then get the go-ahead
The **sprint boundary is the checkpoint**; the sprint interior is autonomous. Don't plow from "what's
this sprint about?" straight into execution — compose the plan and **hard-pause** for a nod first:
1. **Goal** — settle the one-line sprint goal with the user (a short conversation is fine).
2. **Compose** — pick the candidate cards from `wiki/backlog/` and write the plan **into**
   `wiki/sprint.md`: the goal + the task list (slugs) + rough order. `sprint.md` *is* the plan.
3. **Show + wait** — present the composed sprint (goal, the cards = the workload, order) and **wait for
   an explicit go-ahead** before running any card. The user may swap / drop / reorder. This is the one
   blocking checkpoint per sprint — cheap to eyeball, and a mis-composed sprint is the costliest miss.
4. On go-ahead → run the items (**Per item**, below), interior autonomous until blocked.

## Per item

1. **Pull** the card into the sprint task list. The card **is** the spec. (Reading the card triggers
   the retrieval suggester — it lists the relevant atoms.)
2. **Retrieve** — read the suggested + linked knowledge atoms *before touching code*; follow their
   links; always know `wiki/knowledge/project/the-rules.md`. The knowledge gate blocks work until
   current knowledge has been read. Don't re-derive what an atom states.
3. **Right-size** — flesh thin cards with acceptance criteria **in the card itself** (never a separate
   spec file). A task can also be **talked through** with the user first — a conversation is a valid
   path; its conclusions land in the card / atoms, not in chat history. Pause for the user only if the
   card is `flags:[review]` or a real fork appears.
4. **Do it inline.** Right-size the tests per the project's testing atoms (or the-rules): pure logic →
   unit tests; behaviour the suite can't reach → the project's stated verification (often: run it and
   look). Don't add ceremony the project's policy doesn't ask for.
5. **Verify** — the project's check/test commands pass; manual verification done where the policy says so.
6. **Capture** — the capture gate fires at close: write any **durable, reusable** learning as a
   knowledge atom (update the existing atom if one covers the topic — one canonical home; short +
   linked). Routine edits → say so in one line.
7. **Review** — spawn the `reviewer` agent for an independent check. (A trivial, user-eyeballed tweak
   may skip review — say so.)
8. **Close** — delete the card (learnings are now atoms), tick the sprint task, log one line. Commit
   `<slug>: <title>` **only when the user asks**.

## Sprint close — the retro + archive
When the sprint's tasks are done (or the user calls it) — the sprint-close hook nudges you here once
when every task is checked:
- **Retro, briefly**: what went well / what dragged / what recurred. A durable lesson → an atom; a
  recurring failure-shape → flag it in a `wiki/knowledge/project/known-issues.md` atom (create on
  first need); a process fix → propose it to the user.
- Confirm all learnings reached the wiki.
- **Archive the sprint** to `wiki/sprint-archive/NNNN-slug.md` (next number, slug from the goal).
  **Preserve the append-only run log** as the spine and enrich it with the sprint goal, the cards you
  closed, and links to the atoms you captured (don't reconstruct from `git log` + cards alone — that
  drops the in-flight gotcha-and-fix tier only the log carries). The archive is the **write-once
  ledger**: what shipped, when; one bounded file per sprint, never appended to after close.
- Then **clear `wiki/sprint.md`** (run log and task list reset) for the next sprint. Three tiers: the
  wiki keeps the *learnings*, the archive keeps the *ledger* (the preserved narrative), the sprint
  file is the *live working state*.

## Rules
- Knowledge lives in `wiki/knowledge/` atoms — **never** native memory, never the sprint log.
- Links flow **work → knowledge**, never knowledge → work. One canonical home; replace
  (delete + repoint) when wrong.
- New work discovered mid-task → `/intake` a card (slug-named); don't inline-patch.
- Research items → spawn the `researcher` agent (writes atoms, no code).
- Run until blocked **within** an agreed sprint: once the user has OK'd the plan (**Sprint start**),
  keep pulling items until the sprint is done, a `review` card, or a real fork. Composing a *new*
  sprint is itself a boundary — stop and get the go-ahead, don't auto-start the next one.
