# Vibin — operating rules

Vibin is a **seed repo** cloned per project. It runs a **wiki-driven, spec-driven,
test-first** multi-agent workflow. These rules are enforced by hooks in `.claude/` and
by the agent and skill definitions in `.claude/agents/` and `.claude/skills/`.

## The wiki is the single source of truth

- `wiki/` is the spec. There is no separate specs directory — detailed feature specs
  live in `wiki/specs/` as wiki pages.
- `wiki/INDEX.md` is the required entry point. **Every agent reads it first.** This is
  enforced hard: a `PreToolUse` hook blocks writes, Bash, and agent spawns until the
  current `wiki/INDEX.md` has been read in this session/agent.
- The wiki is open-ended. Only `INDEX.md` is structurally required; add, split, and
  restructure other pages freely, and link them from `INDEX.md`.
- When code diverges from the wiki, **update the wiki** (or run `/wiki-sync`). The
  `PostToolUse` reminder will nudge you.

## Workflow

1. `/bootstrap` interviews the user, populates the `wiki/` starter pages, scaffolds the
   chosen stack, and hands off to the `manager` skill.
2. The top-level session runs the `manager` skill: it reads the wiki + `wiki/backlog.md`,
   commits the bootstrap baseline (the scaffold + populated wiki) on its first run,
   presents an ordered work plan for approval, then for each item runs the pipeline:
   `spec-writer` → `test-writer` → `implementer` → `reviewer`.
3. **Tests are always written first.** `test-writer` writes failing tests from the spec
   page and confirms red; `implementer` writes the minimum code to reach green.
4. An item is **done** when the reviewer passes AND the full test suite is green. The
   manager commits one commit per completed item (no push) and loops.

## Operational rules

- **Top-level boundary** — the top-level session answers questions, runs `/bootstrap`,
  and runs the `manager` skill to orchestrate the build. Orchestration lives at the top
  level because only the top-level session can spawn subagents. Even so, the top-level
  session never writes product code, specs, or tests itself — every such artifact goes
  through a delegated subagent.
- **Artifact handoff** — subagents do not share a conversation. They communicate only
  through repo + wiki artifacts. Delegation prompts must name the exact files to read
  and write.
- **Run until blocked** — the manager works through the backlog without per-item
  check-ins, pausing only on a review checkpoint, an unresolved failure, or a reviewer
  escalation.
- **Review checkpoints** — the manager pauses and asks the user directly for approval
  for: (1) the initial work plan, always; (2) any `wiki/backlog.md` item flagged
  `review`. Items are flagged by the user or auto-flagged by the manager when
  risky/ambiguous/architecturally significant. Unflagged items never pause.
- **Retry / escalation** — `implementer` gets 3 attempts to reach green; then control
  returns to the manager for one more routed attempt, then escalation to the user. A
  `reviewer` rejection loops back to `implementer` once with the notes; a second
  rejection escalates.
- **Commits** — one commit per completed item, message references the backlog item.
  Never push unless the user asks.
- **Resumability** — the manager's durable state is `wiki/backlog.md` +
  `wiki/progress.md`. A fresh `/manager` invocation reads those and continues.
- **Decision ownership** — any agent making a notable design/tech choice appends it to
  `wiki/decisions.md` (ADR-style); the manager logs orchestration decisions.
- **Escalation is visible** — when the manager pauses or escalates, it writes the reason
  to `wiki/progress.md` and states it in chat.
- **Specialist agents** — beyond the four pipeline subagents, the manager may spawn
  ad-hoc `general-purpose` specialists (researcher, security-auditor, designer, …) or
  persist recurring ones as `.claude/agents/*.md`. All specialists obey the same rules:
  read the wiki first, hook-gated, artifact handoff.
