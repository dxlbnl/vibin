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
                                                  feature  →  spec → tests → impl → review
                                                  bug      →  spec → tests (incl. regression) → impl → review
                                                  research →  researcher → review
                                                  chore    →  impl → review
                                                → git mv to done/, commit, loop
```

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
| `/tdd-cycle` | Canonical reference for the red-green-refactor discipline (lives in agent prompts; this page settles arguments). |

## Staying in (or out of) the loop

By default the manager runs **until blocked**. You're pulled in for:

- the **initial work plan**, always;
- any item flagged `review` in its card frontmatter (`flags: [review]`) — pauses
  after the spec is written, before tests/impl; and
- **escalations** (retry budget exhausted, second reviewer rejection).

Everything else runs hands-off. Run `/status` any time to peek without disturbing
the pipeline. If new work comes up mid-run, file it with `/intake` rather than
asking the active agent to handle it inline — the new item flows through the normal
spec → tests → review pipeline like everything else.

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

## Rolling back

Each completed backlog item is exactly one commit, and its card lives in
`wiki/backlog/done/` afterwards. To undo an item, `git revert` its commit.
