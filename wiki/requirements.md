# Requirements

> **Starter template.** `/bootstrap` fills this in from your answers. Keep each
> requirement concrete and verifiable — these become the basis for spec pages and tests.

## Functional requirements

*Examples:*

- R1: A user can create a shared list, invite others by email, and see updates in
  real time.
- R2: Each list item records who added it, who bought it, and the cost.
- R3: At any time, a user can see what each person owes everyone else.

## Constraints

Hard limits: platforms, dependencies, performance budgets, compliance, deadlines.
*Examples:*

- Must work on mobile Safari and Chrome (no native apps for v1).
- No third-party analytics; all data stays on the project's own infra.
- First usable demo by end of month.

## Assumptions

Things taken as given. If an assumption proves false, log it in `decisions.md`.
*Examples:*

- Users have a stable network connection while in the app.
- Lists rarely exceed 100 items.

## Open questions

Unresolved questions. The manager may flag related backlog items as `review`.
*Examples:*

- Do we need offline support for v1, or can we defer it?
- How are invited users authenticated — magic link, password, OAuth?
