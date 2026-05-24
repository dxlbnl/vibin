# Changelog

Human-facing history of changes to the Vibin **seed** — its agents, skills, hooks,
operating rules (`CLAUDE.md`), and wiki templates.

Vibin is cloned per project, so seed improvements do **not** reach already-bootstrapped
projects automatically. Each released change has a **migration** in
[`migrations/`](migrations/); run [`/migrate-vibin`](.claude/skills/migrate-vibin/SKILL.md)
to apply the ones a project is missing. A project records the seed commit it is synced to in
`.vibin-version` (a git hash); the seed repo itself ships no such file.

## How to upgrade a project

Ask Claude to **"migrate this project to the latest Vibin"** (or run `/migrate-vibin`). It reads
the seed commit hash in `.vibin-version`, diffs it against the latest Vibin on GitHub
(`dxlbnl/vibin`), and applies the changes — adopting seed-owned files the project hasn't
customized, reconciling ones it has (checking the load-bearing files like agents and hooks),
following each affected migration's content-aware steps for the project's wiki, then updating
`.vibin-version` and committing. See [`migrations/README.md`](migrations/README.md).

## Released versions

| Version | Date | Change | Migration |
|---|---|---|---|
| v1 | 2026-05-22 | Standing decisions propagate via an `architecture.md` **Rules** index (binding RFC-2119 rules; `decisions.md` becomes a standing-constraint rationale archive; the manager owns the Rules index). | [`migrations/0001-rules-index.md`](migrations/0001-rules-index.md) |
| v2 | 2026-05-24 | RFC-2119 spec **requirements**: each carries a stable `B<n>-R<k>` id, one RFC-2119 keyword, and ≥1 `GIVEN/WHEN/THEN` scenario (one scenario → one test); strict Open-Questions gate; manager spec-validation check before `test-writer`. | [`migrations/0002-rfc2119-specs.md`](migrations/0002-rfc2119-specs.md) |

## Proposed / upcoming

Designs not yet released live in [`docs/proposals/`](docs/proposals/). When a proposal
ships it gets a `migrations/` entry and a row in the released table above.

- **0002 — Lite vs full pipeline** — a gated lightweight track so trivial, behavior-neutral
  changes skip the full spec→tests→review ceremony.
  [`docs/proposals/0002-lite-vs-full-pipeline.md`](docs/proposals/0002-lite-vs-full-pipeline.md)
- **0003 — Skill library + anatomy** — a reusable practice-skill library, plus the
  Rationalizations / Red flags / Verification anatomy that hardens agents against shortcuts.
  [`docs/proposals/0003-skill-library-and-anatomy.md`](docs/proposals/0003-skill-library-and-anatomy.md)
- **0004 — interview-me** — a one-question-at-a-time discipline driving `/bootstrap` and
  `/intake` to high confidence before acting.
  [`docs/proposals/0004-interview-me.md`](docs/proposals/0004-interview-me.md)
- **0005 — Browser testing** — opt-in UI verification (Chrome DevTools MCP / Playwright) so
  "tests pass" is backed by "it works in a browser."
  [`docs/proposals/0005-browser-testing.md`](docs/proposals/0005-browser-testing.md)

> Deferred (no proposal yet): **delta / living-per-capability specs** — the real fix for
> behavioral-spec staleness, but a large change worth its own pass only when a project is big
> enough to feel the pain. See the appendix of `0001`.
