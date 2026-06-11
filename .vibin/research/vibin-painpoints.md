# Vibin in practice — a pain-point study

**Status:** research / diagnosis only. No solutions, no redesign here — those wait until
after the second source we're bringing in.
**Evidence base:** `/home/dexter/Projects/typescript/zod4-mock-2`, the most mature Vibin run
to date (120 done items, 36 ADRs, 254 wiki files, ~7.4k LOC src / ~20.7k LOC tests).
**Method:** read-only — wiki artifacts, git history, and the seed's own skill/agent/hook
definitions. Every claim below cites a concrete artifact.

---

## Why this study exists

Vibin works — `zod4-mock` shipped, with end-to-end traceability most teams never achieve.
But running it daily surfaced friction that the success masks. This document fixes the
*problems* in evidence so the redesign that follows is grounded, not guessed. The maintainer's
own framing: git-flow across branches is painful, the loop is slow and cumbersome, the
artifacts are unreadable, the prose volume doesn't pay, there's no room to just *talk through*
a task, and the "wiki" has become a kanban board rather than a knowledge base.

---

## The eleven pains

### P1 — Git-flow across branches clashes; backlog items collide
- IDs are `max(B<n>)+1` over a glob of `wiki/backlog/**` (`intake` SKILL, step 3) → two
  branches mint the same next id → collision on merge.
- Every item appends to the *bottom of the same four files* — `progress.md` (1742 ln),
  `decisions.md` (1290 ln), `architecture.md` Rules, plus a new row in the `INDEX.md` Pages
  table → parallel items conflict on all four, every time.
- Lane moves are `git mv` while the card is simultaneously edited (frontmatter `spec:`, Notes)
  → rename/modify conflicts.
- Symptom in the wild: the `provenance-explorer` branch arc was abandoned mid-way (B88/B86
  done; B87/B90 stranded in inbox).

### P2 — The loop is slow and cumbersome
- A feature/bug runs **four cold subagents** (spec-writer → test-writer → implementer →
  reviewer). Each prompt opens "Read `wiki/INDEX.md`, architecture.md, the spec…" and the
  wiki-gate *forces* it — re-reading a 100-line rules file and (potentially) the 1290-line
  decisions log before any work.
- Serial by design: `doing/` holds "at most one item."
- Ceremony is flat: a chore (B137, delete comments) and a one-line key alias (B122) pay the
  same pipeline as an engine change.
- Retry/escalation loops add still more cold spawns.

### P3 — Specs and artifacts are hard to read
- `wiki/specs/B134-*.md` is **587 lines for a single bug**.
- `progress.md` is 1742 lines of agent→agent ID-soup ("RED — R1/R2 siblings dropped … R9 keys
  are `["number"]` … D33 control green").
- The "one-line" Rules are 80–120-word multi-clause sentences (D36 is one sentence, six
  clauses).

### P4 — The volume of prose doesn't pay off
- B134's 587-line spec argued **R3 — "an override array never resizes"** — which **B136
  reversed two days later**. The most-argued requirement lived 48 hours.
- The same override behaviour was relitigated **four times**: B12 → B53 → B134 → B136.
- The workflow generated its own debt: **two cleanup chores (B137 + B138, net −2194 lines)**
  purely to delete narration the agents had written into `src/`.
- Intent is copied four ways (card → spec → ADR → journal); none is authoritative. The code
  and tests are the real contract.

### P5 — Tasks are forced into ceremony; there's no room for a conversation
- A bug reported in one sentence ("user reported via `playground.ts`, label 'failing
  overrides'"; the spec itself notes "Reported conversationally (no GitHub issue)") becomes a
  587-line book of requirements and decisions.
- The pipeline has exactly four tracks (feature/bug/research/chore), all of which funnel into
  the spec/test/impl/review relay. `intake` forces every report into
  `type/priority/description/flag` first. There is **no "talk it through" mode** — no track
  where a human and the agent reason about a task before (or instead of) generating a
  document.

### P6 — The "wiki" is a glorified kanban board, not a knowledge base
- Of **254 wiki `.md` files, 190 (75%) are per-work-item artifacts**: 134 backlog cards + 56
  spec pages. The remaining 64 still include the journal, the decisions log, and READMEs.
- The genuine knowledge/index pages are **link-dead**: `architecture.md`, `requirements.md`,
  `vision.md`, `codebase-map.md`, and `research/overview.md` each contain **0 `[[wikilinks]]`**.
- The `[[wikilinks]]` that *do* exist cluster in `backlog/done` (15) and `specs` (12) — i.e.
  cross-linking serves **audit traceability between work items**, not knowledge navigation.
- Net: a few long monolithic pages plus a mountain of tracking files — the opposite of the
  LLM-wiki ideal (short pages, dense cross-links). The Rules are genuinely valuable; almost no
  *other* reusable knowledge is captured, and what is captured isn't interlinked.

### P7 — "Definition of done" erodes; known-red tests ride along untracked
- Three `B126-twoslash` tests have failed since ~B130, carried as a prose "separate triage"
  NOTE through **B130, B134, B135, B136, B137, B138** — six items all marked **done** with the
  suite red. No bug card was ever filed for them.

### P8 — Planned multi-item arcs get preempted and rot
- The provenance/explorer arc (B88 → B86 → B87 → B90) stalled after B86; B87 and B90 sit in a
  14-item inbox with nothing tracking that the arc is half-finished.

### P9 — The four-lane backlog is really three lanes
- `wiki/backlog/ready/` contains only `.gitkeep`; the manager pulls straight from inbox on
  verbal direction ("work on theme 1").

### P10 — "Append-only" logs are bent in place
- `decisions.md` says "never edit a past entry — supersede it," yet D14 carries an in-place
  "Amendment", D5 is amended by D24, and B136 edited the D14 Rules line in place.

### P11 — Knowledge is duplicated, inviting drift
- "Vision" exists three times (`wiki/vision.md`, `wiki/product/vision.md`,
  `wiki/site/vision.md`); the wiki-gate only enforces reading `INDEX.md`, so nothing keeps the
  254 files mutually consistent.

---

## Reading of the pattern (diagnosis, not prescription)

Three forces recur underneath the eleven:
1. **State lives in monolithic, globally-numbered, git-tracked files** → every concurrency and
   merge pain (P1, P8–P11).
2. **Ceremony is fixed, not proportional to the task** → every speed and prose pain (P2–P5),
   regardless of whether the change is a typo or an engine rewrite.
3. **Artifacts are written agent→agent for machine precision and audit, then handed to a human
   for comprehension and reuse** → the readability and "not-a-wiki" pains (P3, P6).

## Deliberately not covered here
- Solutions / redesign options.
- "The core of Vibin" — what the minimal essential loop is, and what to rebuild from.
- Lessons from the **second source** the maintainer is bringing in.

These are the next steps, once that source is in hand.
