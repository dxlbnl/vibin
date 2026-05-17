# Vibin — operating rules

Vibin is a **seed repo** cloned per project. It runs a **wiki-driven, spec-driven,
test-first** multi-agent workflow. These rules are enforced by hooks in `.claude/` and
by the agent and skill definitions in `.claude/agents/` and `.claude/skills/`.

## The wiki is the single source of truth

- `wiki/` is the spec. Detailed feature specs live in `wiki/specs/` as wiki pages —
  there is no separate specs directory.
- **Every agent reads `wiki/INDEX.md` first.** A `PreToolUse` hook enforces this so it
  cannot be skipped; if the wiki has changed since you last read it, re-read the
  affected pages before continuing.
- The wiki is open-ended. Only `INDEX.md` is structurally required; add, split, and
  restructure other pages freely, and link them from `INDEX.md`.
- When code diverges from the wiki, **update the wiki** (or run `/wiki-sync`). The
  `PostToolUse` reminder will nudge you.

## Workflow

1. `/bootstrap` interviews the user, populates the `wiki/` starter pages, scaffolds the
   chosen stack, writes the stack-specific permission profile into
   `.claude/settings.json`, and hands off to the `manager` skill.
2. The top-level session runs the `manager` skill: it reads the wiki + the items in
   `wiki/backlog/{inbox,ready}/`, commits the bootstrap baseline (the scaffold +
   populated wiki) on its first run, presents an ordered work plan for approval, then
   for each item dispatches the right track based on the item's `type:`:
   - `feature` → `spec-writer` → `test-writer` → `implementer` → `reviewer`
   - `bug` → same as feature, plus a regression test for the reported failure
   - `research` → `researcher` specialist → reviewer confirms findings
   - `chore` → `implementer` → `reviewer` (no spec, no tests-first)
3. **Tests are always written first** (for `feature` and `bug` items). `test-writer`
   writes failing tests from the spec page and confirms red; `implementer` writes the
   minimum code to reach green.
4. An item is **done** when the reviewer passes AND the full test suite is green. The
   manager `git mv`s the item file to `wiki/backlog/done/`, commits one commit per
   completed item (no push), and loops.

## Operational rules

- **Top-level boundary** — the top-level session answers questions, runs `/bootstrap`,
  and runs the `manager` skill to orchestrate the build. Orchestration lives at the top
  level because only the top-level session can spawn subagents. Even so, the top-level
  session never writes product code, specs, or tests itself — every such artifact goes
  through a delegated subagent.
- **Artifact handoff** — subagents do not share a conversation. They communicate only
  through repo + wiki artifacts. Delegation prompts must name the exact files to read
  and write.
- **Triage** — any bug report, feature request, or change of direction surfaced
  mid-run becomes a new item in `wiki/backlog/inbox/` via `/intake`. Never inline-patch
  in response. The capturing agent files the item, tells the user, and continues the
  current item. The only exception is a trivial typo/comment fix adjacent to the
  current item, which is folded into the current item's commit.
- **No ad-hoc `node`/`python` invocations** — agents must not run `node -e ...`,
  `node <oneoff.js>`, `python -c ...`, `python <oneoff.py>`, or similar interpreter
  scripts as ad-hoc investigation or probing tools. The right tool for each pattern:
  - Searching or inspecting code → `Read` / `Grep` / `Glob` (not a node script).
  - Inspecting data files (JSON, CSV, logs) → `Read` (and `jq` via Bash if needed).
  - Probing an external API to check it works → describe the request (curl / fetch /
    endpoint + body) and **ask the user** to run it.
  - Verifying behaviour of the system being built → write a real test through the
    `test-writer` / `implementer` flow, not a throwaway invocation.
  - Mutating environment, CI, build, or local-tool configuration → describe the
    change (file path + exact diff or shell command) and **ask the user** to apply it.
    Committing a config *file* the project owns — `vite.config.ts`, `pyproject.toml`,
    `Dockerfile`, a CI workflow yaml — is fine; that's product code.

  **Exception**: project-owned commands (`pnpm run …`, `pytest`, `tsc --noEmit`,
  `cargo test`, or a script the project has committed) are fine — those are the
  project's normal operations, not ad-hoc agent work.
- **Package manager** — always use the one declared in `wiki/architecture.md`. Do not
  substitute another even if generated configs, READMEs, or model priors suggest one.
  If the declaration is missing or ambiguous, defer to the user.
- **Run until blocked** — the manager works through the backlog without per-item
  check-ins, pausing only on one of three things:
  1. A **review checkpoint** (the initial work plan, or any item flagged `review`).
  2. An **unresolved failure** (retry budget exhausted — see below).
  3. A **reviewer escalation** (second rejection on the same item).
- **Review checkpoints** — the manager pauses and asks the user directly for approval
  for: (1) the initial work plan, always; (2) any item flagged `review` in its card
  frontmatter (`flags: [review]`). Items are flagged by the user or auto-flagged by the
  manager when risky/ambiguous/architecturally significant. Unflagged items never pause.
- **Retry / escalation** — `implementer` gets 3 attempts inside its own loop to reach
  green. If still red, the manager routes the failure context back for one more
  attempt (4th total), then escalates to the user. A `reviewer` rejection loops back to
  `implementer` once with the review notes; a second rejection escalates.
- **Resuming and unblocking** — to resume after a pause, run `/manager` (or `/status`
  to inspect first). To skip an escalated item, edit its frontmatter to add
  `flags: [blocked]` and a one-line reason in `## Notes`, then re-run `/manager`. To
  cancel, `git mv` the item to `wiki/backlog/done/` and add `flags: [cancelled]`.
- **Commits** — one commit per completed item, message references the backlog item id
  (e.g. `B3: add user login`). Never push unless the user asks.
- **Resumability** — the manager's durable state is `wiki/backlog/**` +
  `wiki/progress.md`. A fresh `/manager` invocation reads those and continues.
- **Decision ownership** — any agent making a notable design/tech choice appends it to
  `wiki/decisions.md` (ADR-style); the manager logs orchestration decisions.
- **Escalation is visible** — when the manager pauses or escalates, it writes the reason
  to `wiki/progress.md` and states it in chat.
- **Specialist agents** — beyond the four pipeline subagents, the manager may spawn
  ad-hoc `general-purpose` specialists (researcher, security-auditor, designer, …) or
  persist recurring ones as `.claude/agents/*.md`. All specialists obey the same rules:
  read the wiki first, hook-gated, artifact handoff.
