# Specs

Detailed feature specs live here — **one page per feature**, created and refined by the
`spec-writer` agent from a `backlog.md` item. These are wiki pages, not a separate source
of truth: the wiki *is* the spec.

Each spec page is named after its backlog item (e.g. `B1-user-login.md`) and linked from
`../INDEX.md`.

## Spec page format

```
# B<n>: <feature title>

## Context
<why this feature exists; link to the relevant wiki pages>

## Acceptance criteria
<numbered, TESTABLE statements — these map directly to tests>
1. ...
2. ...

## Out of scope
<what this feature deliberately does not cover>

## Open questions
<anything unresolved; if blocking, the item should be flagged `review` in backlog.md>
```

The acceptance criteria are the contract: `test-writer` turns each one into failing
tests, `implementer` makes them pass, `reviewer` checks every criterion is met.
