# Proposals

A **proposal** is a design doc for a change to the Vibin **seed** — its agents, skills,
hooks, operating rules (`CLAUDE.md`), and wiki templates. It captures the motivation,
design, and open questions *before* the change is built.

A proposal is **not** a migration. A [migration](../../migrations/README.md) is a *released*
seed change with agent-runnable steps; the human-facing list of released migrations is
[`CHANGELOG.md`](../../CHANGELOG.md). A proposal becomes a migration when it ships.

This page is the **authoritative index of proposal status** — the tables below say what is
proposed vs implemented.

## Lifecycle

```
Proposed                      Implemented
docs/proposals/NNNN-*.md  ─►  docs/proposals/implemented/NNNN-*.md
(Status: Proposed)            (Status: Implemented — ships as migration MMMM)
```

- A proposed design lives at the top level of `docs/proposals/` with
  `Status: Proposed (not yet implemented)`.
- When it ships, it is implemented, flipped to `Status: Implemented — ships as migration MMMM`,
  and **moved** into `docs/proposals/implemented/` (proposal ids and migration ids are
  independent sequences — they will not line up).

> Note: `docs/**` is **seed-meta** — it documents Vibin's own evolution and is never copied
> into a child project by `/migrate-vibin`. Reorganizing proposals needs no migration.

## Proposed

| # | Proposal | Summary |
|---|---|---|
| 0002 | [Lite vs full pipeline](0002-lite-vs-full-pipeline.md) | A gated lite track so trivial, behavior-neutral product changes skip the full spec→tests→review ceremony. |
| 0003 | [Skill library + anatomy](0003-skill-library-and-anatomy.md) | Rationalizations / Red-flags / Verification anatomy that hardens agents against shortcuts, plus a reusable practice-skill library. |
| 0004 | [interview-me](0004-interview-me.md) | One-question-at-a-time interviewing driving `/bootstrap` and `/intake` to high confidence before acting. |
| 0005 | [Browser testing](0005-browser-testing.md) | Opt-in UI verification (Chrome DevTools MCP / Playwright) so "tests pass" is backed by "it works in a browser." |

## Implemented

| # | Proposal | Shipped as |
|---|---|---|
| 0001 | [RFC-2119 specs](implemented/0001-rfc2119-specs.md) | [migration 0002](../../migrations/0002-rfc2119-specs.md) |

## Shipping a proposal (checklist)

When a proposal is implemented, do these in order (this is exactly how 0001 shipped):

1. **Implement the design** — make the seed edits the proposal's "Files the implementation
   will touch" section lists.
2. **Add the migration** — `migrations/NNNN-<slug>.md` with `## Detect` / `## Apply` /
   `## Verify`, `seed_commit` set to the implementing commit. **Verify checks must be
   read-only / tool-checkable (`Read`/`Grep`/`Glob`), never shell** — see
   [`migrations/README.md`](../../migrations/README.md).
3. **CHANGELOG** — add a released-versions row in [`CHANGELOG.md`](../../CHANGELOG.md), and
   drop the proposal from its "Proposed / upcoming" list.
4. **Flip Status** — set the proposal's header to
   `Status: Implemented — ships as migration NNNN`.
5. **Move it** — `git mv docs/proposals/NNNN-<slug>.md docs/proposals/implemented/` and fix
   any cross-links broken by the move.
6. **Update this index** — move the proposal's row from **Proposed** to **Implemented** above.

## Roadmap

Recommended order for the proposed designs. Each ships one-by-one via the checklist above;
each carries open questions to settle at its own implementation time.

1. **0003 Phase 1 — skill anatomy** *(first up)*. Add Rationalizations / Red-flags /
   Verification sections to `test-writer`, `implementer`, `reviewer` (and lightly
   `spec-writer`), cross-referenced from `tdd-cycle`. Cheap, high value, mostly prompt edits,
   independent of the others. No real fork to settle.
2. **0004 — interview-me**. The one-adaptive-question discipline for `/bootstrap` Phase 1 and
   `/intake`. Fork to settle: a standalone `.claude/skills/interview/SKILL.md` vs a shared
   conventions section (the proposal leans standalone skill).
3. **0002 — lite vs full pipeline**. The gated lite track across the manager, `/intake`,
   `reviewer`, and the backlog schema. Forks: `mode: lite` flag vs a new `type:` (leans
   flag); the auto-promote-to-full rule.
4. **0003 Phase 2 — practice-skill library** (`.claude/skills/practices/**`). Larger; builds
   on Phase 1's anatomy. Forks: where practices live / how they register; manager-driven vs
   agent-pulled loading (leans manager-driven); how much ships in the seed.
5. **0005 — browser testing** *(last; largest, most uncertain)*. Opt-in UI verification wired
   by `/bootstrap` for frontend stacks. Big fork: Chrome DevTools MCP vs committed Playwright
   (possibly both); a separate stage vs folded into `reviewer`; how a spec marks "has UI".
