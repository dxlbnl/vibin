# Vibin

A **seed repo** for starting any new project with a **wiki-driven, spec-driven,
test-first** multi-agent workflow. Clone it, run `/bootstrap`, and a team of agents
builds your project from the wiki.

## How it works

The `wiki/` directory is the **single source of truth** — the wiki *is* the spec. The
top-level session runs a `manager` skill that reads the wiki, decides what to build,
and orchestrates a pipeline of specialized subagents. The pipeline used for each
backlog item depends on the item's `type:`.

```
/bootstrap  →  wiki/ populated  →  manager  →  per item in wiki/backlog/:
                                                dispatch on item type:
                                                  feature              →  spec → tests → impl → review
                                                  bug                  →  spec → tests (incl. regression) → impl → review
                                                  research             →  researcher → review
                                                  chore                →  impl → review
                                                  feature/bug + lite   →  impl → review (lite)   ← gated: behavior-neutral changes only
                                                → git mv to done/, commit, loop
```

Cross-cutting machinery the manager uses while running an item:

- **Spec open questions are answered or deferred** — never silently dropped. A blocking
  question bounces the card to `wiki/backlog/inbox/` with `flags: [needs-answers]`; the
  manager keeps working other items; you answer on the card and it resumes.
- **Practices** — `.claude/practices/<name>.md` (security, accessibility, debugging,
  performance, copywriting, browser-testing) are loaded on demand into the relevant
  delegation prompts; never the whole library.
- **Browser / UI verification** — opt-in for frontend projects (`/bootstrap` enables it).
  Spec `Scenario (UI):` entries get a committed Playwright test plus a Chrome DevTools MCP
  drive-and-screenshot at review.
- **Interview discipline** — `/bootstrap` and `/intake` use a one-question-at-a-time
  discipline to reach ~95% confidence before writing the wiki or an item card.

Items live as per-item files under `wiki/backlog/<lane>/` (lanes: `inbox/` → `ready/`
→ `doing/` → `done/`). The manager moves cards with `git mv`, preserving history.

## Getting started

1. Clone this repo for your new project.
2. Run `/bootstrap`. It interviews you about: the project (vision, users, success,
   non-goals); the **stack** (TypeScript / Python / Rust / Go / Other) and, for
   TypeScript, the **package manager** (`pnpm` is the default and recommended;
   `npm` / `yarn` are available but flagged); testing setup; constraints; and the
   first concrete backlog items. It then writes `wiki/` starter pages, scaffolds the
   stack, and appends the stack-specific permission profile to
   `.claude/settings.json`.
3. Review/refine the `wiki/` pages (it's open-ended — add whatever pages you want).
4. The `manager` presents an ordered work plan for your approval, then builds the
   backlog autonomously.

## Commands

| Command | Use for |
|---|---|
| `/bootstrap` | First-time project setup. Interview, populate the wiki, scaffold the stack, hand off to the manager. |
| `/manager` | Drive the build. Run this after `/bootstrap`, and again any time you want to resume after a pause or escalation. |
| `/intake "<title>"` | File a new bug, feature, research, or chore item into `wiki/backlog/inbox/`. Use this for anything that comes up mid-run instead of letting an agent inline-patch. |
| `/status` | One-screen summary of where the pipeline is right now: lane counts, the active item with its current stage, blocked items, the last escalation, the next three ready items. Read-only. |
| `/wiki` | Conventions reference for reading and maintaining the wiki. |
| `/wiki-sync` | Reconcile the wiki with the current code when the implementation has drifted (or the `PostToolUse` reminder nudges you). |
| `/migrate-vibin` | Bring an existing project up to the latest seed by diffing its `.vibin-version` against the latest Vibin and applying the changes (see "Upgrading an existing project"). |
| `/tdd-cycle` | Canonical reference for the red-green-refactor discipline (lives in agent prompts; this page settles arguments). |

## Staying in (or out of) the loop

By default the manager runs **until blocked**. You're pulled in for:

- the **initial work plan**, always;
- any item flagged `review` in its card frontmatter (`flags: [review]`) — pauses
  after the spec is written, before tests/impl;
- **escalations** (retry budget exhausted, second reviewer rejection); and
- items the manager bounced to `wiki/backlog/inbox/` with `flags: [needs-answers]` — these
  are waiting on your answers to **blocking open questions** written on the card. Answer in
  the card and clear the flag (or move to `ready/`), and the item resumes from the spec.

Everything else runs hands-off. Run `/status` any time to peek without disturbing
the pipeline. If new work comes up mid-run, file it with `/intake` rather than
asking the active agent to handle it inline — the new item flows through the normal
spec → tests → review pipeline like everything else. An **answer to an open question** on
the item the manager is actively working is *not* new work — it resolves the current spec
(re-dispatched to `spec-writer`), never filed via `/intake`.

## Stack and package manager

Bootstrap asks for your stack and pins the package manager in
`wiki/architecture.md` (under a section explicitly marked **binding**). For
TypeScript projects, **pnpm is the default**. Agents will only use the package
manager declared there; other package managers aren't added to the allowlist, so
accidental use of `npm` or `yarn` prompts you.

## Decisions and rules

Two files split the "what" from the "why", because a decision is only useful if the next
session actually reads it:

- **`wiki/architecture.md` → Rules** is the binding index of standing constraints (which
  library to use, a pattern code must follow, an architectural boundary). Agents read
  this page before writing code, so this is where a convention has to live to actually
  stick. Rules are one line each, RFC-2119 (**MUST** / **SHOULD** / **MAY**), and link to
  the decision that justifies them.
- **`wiki/decisions.md`** is the append-only rationale archive (ADR-style): the full
  "why" behind each rule, never edited — superseded and relinked. Local, one-off choices
  don't go here; they stay in `progress.md`.

When a new standing convention is adopted — say, "use the component library, don't
hand-roll UI" — it's logged as a decision, the reviewer flags it, and the manager
surfaces it as a rule before the item is done. That's what stops the next session from
quietly reverting to the old pattern.

## What agents won't do

Agents in this seed repo are constrained on purpose. They **will not**:

- Run ad-hoc `node` or `python` scripts to inspect data, poke around, or probe
  external APIs. For inspection they use `Read`/`Grep`/`Glob`; for API checks they
  describe the exact `curl` and ask you to run it.
- Mutate your environment, CI, or local tools via throwaway scripts. They describe
  the change (file path + diff or command) and ask you to apply it. Committing a
  config *file* the project owns — `vite.config.ts`, `pyproject.toml`, a CI
  workflow yaml — is fine; running an imperative setup script is not.
- Inline-patch in response to mid-run bug reports or new requests. Those go through
  `/intake` and the normal pipeline.

Project-owned commands (`pnpm run …`, `pytest`, `tsc --noEmit`, `cargo test`, and
scripts the project has committed) are fine — those are the project's normal
operations.

## The manager and the agents

The **`manager`** is a *skill* the top-level session runs — not a subagent. It
orders the work, dispatches each item to its track, commits completed items, and
never writes product code itself. Orchestration has to live at the top level because
only the top-level session can spawn subagents. The manager delegates to four
pipeline **subagents**:

| Agent | Role |
|-------|------|
| `spec-writer` | Turns a feature/bug item into a testable spec page in `wiki/specs/`. |
| `test-writer` | Writes failing tests from the spec (tests first). Bug items get a regression test. |
| `implementer` | Writes the minimum code to make tests pass. |
| `reviewer` | Verifies the implementation against the wiki and runs the full suite. |

The manager can also spawn **project-specific specialists** (researcher,
frontend-dev, security-auditor, designer, …) ad hoc, or persist recurring ones as
`.claude/agents/*.md` files written by `/bootstrap`.

## Resuming and unblocking

- **Resume after a pause**: run `/manager`. It reconstructs state from
  `wiki/backlog/` + `wiki/progress.md` and continues.
- **Skip an escalated item**: edit the item card's frontmatter to add
  `flags: [blocked]` and a one-line reason in `## Notes`, then re-run `/manager`.
- **Cancel an item**: `git mv` it to `wiki/backlog/done/` and add
  `flags: [cancelled]`.

## Enforcement

- `.claude/hooks/wiki-gate.py` (`PreToolUse`) **blocks** any agent from writing
  files inside the project, running non-trivial Bash, or spawning agents until it
  has read the current `wiki/INDEX.md`. It's deliberately narrow: writes outside
  the project pass through, and read-only Bash (`ls`, `pwd`, `cat`, `head`, `tail`,
  `grep`, `find`, `rg`, `wc`, `git status`/`log`/`diff`/`show`/`branch`) does not
  trip it. If the wiki has changed since you last read it, the gate distinguishes
  ("stale") from "you never read it" in its block message.
- `.claude/hooks/wiki-context.py` (`SessionStart`) injects `wiki/INDEX.md` into
  every session.
- `.claude/hooks/wiki-sync-reminder.py` (`PostToolUse`) nudges to keep the wiki in
  sync when product code changes.

## Upgrading an existing project

Vibin is cloned per project, so seed improvements don't reach existing projects
automatically. A project records the seed commit it is synced to in `.vibin-version` (a git
hash, stamped at bootstrap), and each released change ships as a migration in
[`migrations/`](migrations/). To bring an older project up to date, ask Claude to *"migrate
this project to the latest Vibin"* (or run `/migrate-vibin`): it diffs `.vibin-version`
against the latest Vibin on GitHub, adopts seed-owned files the project hasn't customized,
reconciles ones it has, runs the content-aware steps of any migrations the diff added, then
updates `.vibin-version` and commits. [`CHANGELOG.md`](CHANGELOG.md) is the human-readable
index of releases.

## Rolling back

Each completed backlog item is exactly one commit, and its card lives in
`wiki/backlog/done/` afterwards. To undo an item, `git revert` its commit.
