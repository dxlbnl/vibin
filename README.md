# Vibin

A **seed repo** for spinning up random projects with a **wiki-driven, spec-driven,
test-first** multi-agent workflow. Clone it, run `/bootstrap`, and a team of agents
builds your project from the wiki.

## How it works

The `wiki/` directory is the **single source of truth** — the wiki *is* the spec. The
top-level session runs a `manager` skill that reads the wiki, decides what to build, and
orchestrates a pipeline of specialized subagents. Tests are written before
implementation, always.

```
/bootstrap  →  wiki/ populated  →  manager  →  per backlog item:
                                                spec-writer  (writes wiki/specs/<feature>.md)
                                                test-writer  (writes FAILING tests)
                                                implementer  (makes tests pass)
                                                reviewer     (verifies vs the wiki)
                                                → commit, mark done, loop
```

## Getting started

1. Clone this repo for your new project.
2. Run `/bootstrap` — it interviews you about the project, stack, constraints, and
   initial backlog, then writes the `wiki/` starter pages and scaffolds the stack.
3. Review/refine the `wiki/` pages (it's open-ended — add whatever pages you want).
4. The `manager` presents an ordered work plan for your approval, then builds the
   backlog autonomously.

## Staying in (or out of) the loop

By default the manager runs **until blocked**. You're pulled in for:
- the **initial work plan**, always; and
- any backlog item flagged `review` in `wiki/backlog.md` (you flag it, or the manager
  auto-flags risky/ambiguous items) — it pauses after the spec is written, before
  implementation.

Everything else runs hands-off.

## The manager and the agents

The **`manager`** is a *skill* the top-level session runs — not a subagent. It orders
the work, orchestrates the pipeline, commits, and never writes product code itself.
Orchestration has to live at the top level because only the top-level session can spawn
subagents. The manager delegates to four pipeline **subagents**:

| Agent | Role |
|-------|------|
| `spec-writer` | Turns a backlog item into a testable spec page in `wiki/specs/`. |
| `test-writer` | Writes failing tests from the spec (tests first). |
| `implementer` | Writes the minimum code to make tests pass. |
| `reviewer` | Verifies the implementation against the wiki and the full test suite. |

The manager can also bring in **project-specific specialists** (researcher,
frontend-dev, security-auditor, designer, …) as a project needs them.

## Enforcement

A `PreToolUse` hook (`.claude/hooks/wiki-gate.py`) **blocks** any agent from writing
files, running Bash, or spawning agents until it has read the current `wiki/INDEX.md`.
A `SessionStart` hook injects the wiki index into every session. A `PostToolUse` hook
nudges to keep the wiki in sync when product code changes.

## Rolling back

Each completed backlog item is exactly one commit. To undo an item, `git revert` its
commit.
