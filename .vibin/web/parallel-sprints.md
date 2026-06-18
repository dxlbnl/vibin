---
title: Parallel sprints
type: mechanism
status: accepted
---

Multiple sprints run concurrently by isolating each in its own git worktree on its own branch. An
**orchestrate** skill partitions the backlog, sets up worktrees, and spawns a **sprint-runner**
agent per sprint. Each agent executes its sprint autonomously, commits per card, and reports back.
The orchestrator merges when done.

## Architecture

```
/orchestrate
├── Read backlog → propose partition → pause for go-ahead
├── For each sprint:
│   ├── git checkout -b sprint/<slug>
│   ├── git worktree add .vibin/sprint/<slug> sprint/<slug>
│   ├── Write wiki/sprint.md + copy cards into worktree
│   └── Spawn sprint-runner agent [background]
└── On done: review report → show diff → merge → cleanup

sprint-runner agent (per worktree)
├── Read wiki/knowledge/index.md (gate)
├── Follow run skill: per-card retrieve → do → verify → capture → commit → close
├── Sprint close: retro → archive → commit
└── Report: cards closed, atoms captured, blockers, branch name
```

## The three skills / agents

- **`orchestrate`** (skill) — the launcher: partition, worktree setup, spawn, merge.
- **`sprint-runner`** (agent) — the executor: autonomous, worktree-scoped, reports back.
  Defined in `.claude/agents/sprint-runner.md` so it's a first-class agent with scoped tools,
  not an inline prompt.
- **`run`** (skill) — the canonical execution spec: retrieve → do → verify → capture → commit →
  close → sprint-close. Used by sprint-runner directly, and by `loop` after planning.

**`loop`** remains the human-driven entry point: it plans the sprint (compose + go-ahead) then
follows the `run` skill for execution. No conditionals — planning and execution are separate.

## Key design decisions

**Cards are partitioned before worktrees are created.** The orchestrator assigns cards to sprints
upfront and pauses for user go-ahead — subagents never touch the shared backlog during execution.

**Worktree location.** `.vibin/sprint/<slug>` — add `.vibin/sprint/` to `.gitignore`. Worktrees
are not tracked; they're ephemeral working directories for the duration of the sprint.

**Gate is worktree-aware.** `wiki-gate.py` calls `git worktree list --porcelain` to enumerate all
active worktree roots and accepts reads/writes under any of their `wiki/knowledge/` directories.
This means sprint-runner agents can satisfy the gate from within their own worktree without needing
to read the main project's copy. See [gates](gates.md).

**`--no-ff` merge** preserves sprint history as a visible group in `git log`. Merge one sprint at
a time; resolve wiki atom conflicts (semantic duplicates — one canonical home) before the next.

**Commit discipline.** Every card gets its own commit; sprint close gets its own commit. `git add -A`
is never used — parallel worktrees share the repo and a blanket add sweeps concurrent changes.

**Why:** the loop skill was designed for human-in-the-loop interactive use. Making it serve
autonomous subagents via conditionals produces a fragile hybrid. The clean separation —
`loop` = plan + run, `sprint-runner` = run in a worktree — means each path is unambiguous.
