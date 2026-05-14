# Wiki Index

**This wiki is the single source of truth for the project. It is the spec.**
Every agent reads this page first, before doing anything else.

## How the workflow uses this wiki

- The `manager` agent reads `backlog.md` to decide what to build next.
- `spec-writer` turns a backlog item into a detailed, testable page under `specs/`.
- `test-writer` writes failing tests from that spec page; `implementer` makes them pass;
  `reviewer` verifies the result against this wiki.
- When code and wiki disagree, the **wiki wins** — update the wiki (or run `/wiki-sync`).

## Pages

| Page | Purpose |
|------|---------|
| [vision.md](vision.md) | What the project is and why it exists. |
| [requirements.md](requirements.md) | Functional requirements and constraints. |
| [architecture.md](architecture.md) | Tech stack, structure, and key technical choices. |
| [backlog.md](backlog.md) | Prioritized work items with status and `review` flags. |
| [decisions.md](decisions.md) | Append-only decision log (ADR-style). |
| [progress.md](progress.md) | Append-only run journal — what the agents have done. |
| [specs/](specs/) | One detailed spec page per feature. See `specs/README.md`. |

> The wiki is **open-ended**. Only this `INDEX.md` is structurally required. Add, split,
> and restructure pages as the project grows — just link new pages in the table above.

## Conventions

- **Adding a page**: create `wiki/<name>.md` (or `wiki/specs/<feature>.md`) and add a row
  to the Pages table above so it is discoverable.
- **Backlog status**: each item in `backlog.md` is `todo` / `in-progress` / `done`, with
  an optional `review` flag meaning "pause for user approval before implementing".
- **Spec pages**: live in `specs/`, one per feature, named after the backlog item. They
  must contain testable acceptance criteria — see `specs/README.md`.
- **Decisions**: when an agent makes a notable design/tech choice, it appends an entry to
  `decisions.md`.
- **Progress**: the `manager` appends to `progress.md` as items move through the
  pipeline, so the run is auditable.
