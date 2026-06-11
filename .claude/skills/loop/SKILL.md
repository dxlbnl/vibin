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
- **Resuming**: read `wiki/sprint.md` first — it says what's in flight. No sprint file or all tasks
  done → start/refill the sprint by pulling cards from `wiki/backlog/` (ask the user which, unless
  obvious by priority).
- **Log as you go**: append one-liners to the run log at meaningful steps — it's scratch for
  resumability, freely trimmable, deleted at sprint close. Knowledge does NOT live here.

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

## Sprint close — the retro
When the sprint's tasks are done (or the user calls it):
- **Retro, briefly**: what went well / what dragged / what recurred. A durable lesson → an atom; a
  recurring failure-shape → flag it in a `wiki/knowledge/project/known-issues.md` atom (create on
  first need); a process fix → propose it to the user.
- Confirm all learnings reached the wiki, then **trim the run log to nothing** and clear the task
  list. The wiki is the keeper; the sprint is scratch.

## Rules
- Knowledge lives in `wiki/knowledge/` atoms — **never** native memory, never the sprint log.
- Links flow **work → knowledge**, never knowledge → work. One canonical home; replace
  (delete + repoint) when wrong.
- New work discovered mid-task → `/intake` a card (slug-named); don't inline-patch.
- Research items → spawn the `researcher` agent (writes atoms, no code).
- Run until blocked: keep pulling items until the sprint is done, a `review` card, or a real fork.
