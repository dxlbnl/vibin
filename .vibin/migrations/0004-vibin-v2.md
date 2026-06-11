# 0004 — Vibin v2: the knowledge-graph workflow

The seed moved from the v1 spec-driven manager pipeline (spec-writer → test-writer → implementer →
reviewer, lane-based backlog, monolithic wiki pages) to **v2**: a knowledge graph with enforced
retrieval + capture, slug-named transient work cards, a sprint file, and inline work via the `loop`
skill. Design rationale: `.vibin/web/` in the seed (start at `index.md`). Proven on a live project
(Fosfor/visuals) before landing here.

## Apply — machinery (mostly automatic)

The planner applies/stages `.claude/**` and `CLAUDE.md` as usual. Notes:
- v1 files that should be **deleted** in the project (the planner never auto-deletes): agents
  `spec-writer.md`, `test-writer.md`, `implementer.md`; skills `manager/`, `tdd-cycle/`, `wiki/`,
  `wiki-sync/`; hook `wiki-sync-reminder.py` — **replace this hook with the seed's no-op stub instead
  of deleting it**, and only delete it after a session restart: the harness caches hook config per
  session, and a missing script hard-blocks every tool.
- Adjust `CODE_DIRS` in `.claude/hooks/capture.py` to the project's code dirs.

## Apply — project content (judgment work)

1. **Distil the v1 wiki into knowledge atoms** under `wiki/knowledge/<group>/` (format in
   `wiki/knowledge/index.md`):
   - **Keep** durable knowledge; carry the binding Rules over **verbatim** into
     `project/the-rules.md`.
   - **Drop** audit narration (`progress.md`, journal entries, per-item history) — that was scratch.
   - **Fold research verdicts** into atoms; drop the long survey prose.
   - **Honour supersessions**: an overruled decision/recommendation must NOT survive as guidance —
     the surviving truth gets the atom; note the overruled path inside it if instructive.
   - Sources may be deleted afterwards, so each atom's **Why** carries the rationale inline.
2. **Flatten the backlog**: lanes (`inbox/ready/doing/done`) → flat `wiki/backlog/`; delete `done/`
   cards (their learnings go to atoms if durable). Fold any `wiki/specs/` pages **into their cards**
   (card-is-spec), then delete `specs/`.
3. **Sweep the surviving cards**: remove references to deleted artifacts (specs, lanes, decision
   numbers) and to **overruled designs**; link the relevant atoms instead.
4. Create `wiki/sprint.md` from the seed template. Replace `wiki/INDEX.md` with the v2 template,
   adapted.
5. Delete the v1 monoliths once distilled: `architecture.md`, `decisions.md`, `progress.md`,
   `requirements.md`, `vision.md`.

## Verify (read-only)

- `wiki/knowledge/index.md` exists and lists the atoms; `wiki/knowledge/project/the-rules.md` carries
  the old binding rules.
- No `wiki/backlog/{inbox,ready,doing,done}/` dirs; no `wiki/specs/`; no v1 monolith pages.
- Grep: no card references a deleted spec/lane or an overruled decision.
- `.claude/skills/manager/` gone; `loop`/`intake`/`status` present; `capture.py` + `retrieve.py`
  present; settings wire Stop/SubagentStop → capture.
