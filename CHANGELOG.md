# Changelog

Human-facing history of changes to the Vibin **seed** — its agents, skills, hooks,
operating rules (`CLAUDE.md`), and wiki templates.

Vibin is cloned per project, so seed improvements do **not** reach already-bootstrapped
projects automatically. Each released change has an **executable migration** in
[`migrations/`](migrations/); run [`/migrate`](.claude/skills/migrate/SKILL.md) to apply the
ones a project is missing. A project's current seed version is recorded in `.vibin-version`.

## How to upgrade a project

Ask Claude to **"migrate this project to the latest Vibin"** (or run `/migrate`). It reads
`.vibin-version`, applies every newer migration in `migrations/` in order, bumps the
version, and commits. See [`migrations/README.md`](migrations/README.md) for the mechanism.

## Released versions

| Version | Date | Change | Migration |
|---|---|---|---|
| v1 | 2026-05-22 | Standing decisions propagate via an `architecture.md` **Rules** index (binding RFC-2119 rules; `decisions.md` becomes a standing-constraint rationale archive; the manager owns the Rules index). | [`migrations/0001-rules-index.md`](migrations/0001-rules-index.md) |

## Proposed / upcoming

Designs not yet released live in [`docs/proposals/`](docs/proposals/). When a proposal
ships it gets a `migrations/` entry and a row above.

- **RFC-2119 specs** — normative spec requirements with IDs + `GIVEN/WHEN/THEN` scenarios,
  strict Open-Questions handling, and a manager validation gate.
  [`docs/proposals/0001-rfc2119-specs.md`](docs/proposals/0001-rfc2119-specs.md)
