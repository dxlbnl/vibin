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
- A **phased** proposal ships one migration per phase and records each in its Status; it stays
  at the top level (still "Proposed") until **all** phases are done, then moves to
  `implemented/`. (Proposal 0003 is mid-flight: Phase 1 shipped, Phase 2 pending.)

> Note: `docs/**` is **seed-meta** — it documents Vibin's own evolution and is never copied
> into a child project by `/migrate-vibin`. Reorganizing proposals needs no migration.

## Proposed

| # | Proposal | Summary |
|---|---|---|
| _none_ | — | All proposals to date are implemented. New designs land here as `docs/proposals/NNNN-*.md`. |

## Implemented

| # | Proposal | Shipped as |
|---|---|---|
| 0001 | [RFC-2119 specs](implemented/0001-rfc2119-specs.md) | [migration 0002](../../migrations/0002-rfc2119-specs.md) |
| 0002 | [Lite vs full pipeline](implemented/0002-lite-vs-full-pipeline.md) | [migration 0008](../../migrations/0008-lite-pipeline.md) |
| 0003 | [Skill library + anatomy](implemented/0003-skill-library-and-anatomy.md) | [migration 0004](../../migrations/0004-skill-anatomy.md) (Phase 1) + [migration 0006](../../migrations/0006-practice-library.md) (Phase 2) |
| 0004 | [interview-me](implemented/0004-interview-me.md) | [migration 0007](../../migrations/0007-interview-me.md) |
| 0005 | [Browser testing](implemented/0005-browser-testing.md) | [migration 0009](../../migrations/0009-browser-testing.md) |

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

**No proposals outstanding** — all of 0001–0005 are implemented:

- 0001 (RFC-2119 specs → migration 0002)
- 0002 (lite pipeline → migration 0008)
- 0003 (skill anatomy → migration 0004, practice library → migration 0006)
- 0004 (interview-me → migration 0007)
- 0005 (browser/UI verification → migration 0009)

Out-of-band fix not from a proposal: open-questions resolution → migration 0005.

The only design still parked (no proposal yet) is **delta / living-per-capability specs** —
the real fix for behavioral-spec staleness; see the appendix of proposal 0001. New designs
start as `docs/proposals/NNNN-*.md` and ship via the checklist above.
