# Vibin

A seed repo for **knowledge-driven, agentic development** with Claude Code. Clone it per project.

Vibin v2's core idea: **a knowledge base that builds software** — not a build pipeline that keeps
notes. Agents work a backlog; every task **retrieves** relevant knowledge before working and
**captures** durable learnings after — enforced by hooks, not goodwill — so the project's wiki
compounds: each task makes the next one cheaper.

## How it works

```
retrieve → do (right-sized) → verify → capture → review → close
```

- **Knowledge graph** (`wiki/knowledge/`) — short, linked atoms; one idea, one canonical home.
  The binding constraints live in `project/the-rules.md`.
- **Work items** (`wiki/backlog/`) — slug-named cards; the card *is* the spec (proportional detail).
  Transient: deleted when done, learnings extracted to atoms.
- **Sprint** (`wiki/sprint.md`) — active tasks + a disposable run log. The wiki survives; the log
  is scratch.
- **Gates (hooks)** — *knowledge gate*: work is blocked until the current knowledge is read;
  *capture gate*: a task can't finish without distilling its learnings; *retrieval suggester*:
  reading a card surfaces the relevant atoms automatically.
- **Lean agent set** — work runs inline in the main session via the `loop` skill; only `reviewer`
  (independent check) and `researcher` (research → atoms) are separate agents.

## Quickstart

1. Clone (or copy) this repo as your new project; optionally wipe `.git`.
2. Open it in Claude Code and run **`/bootstrap`** — it interviews you, seeds the knowledge graph,
   scaffolds your stack, sets permissions, strips Vibin's own meta, and stamps `.vibin-version`.
3. Build with **`/loop`**. File new ideas any time with **`/intake`**. Check state with **`/status`**.

## Updating a project

Run **`/migrate-vibin`** — it diffs your project's `.vibin-version` against the latest seed and
applies/stages the changes (machinery auto-applied when safe; your wiki is never auto-touched).

## Repo layout

- `.claude/` — the machinery: hooks (gates), skills (`loop`, `intake`, `status`, `bootstrap`,
  `migrate-vibin`), agents (`reviewer`, `researcher`).
- `wiki/` — the per-project wiki **template** (populated by `/bootstrap`).
- `CLAUDE.md` — the operating rules agents follow.
- `.vibin/` — **Vibin's own development material** (design wiki at `.vibin/web/`, migrations,
  changelog, research). Stripped from projects by `/bootstrap`; never propagated by `/migrate-vibin`.

## v1 → v2

v1 (spec-driven manager pipeline) is superseded. The design rationale lives in `.vibin/web/`
(start at `index.md`); the migration path for existing v1 projects is
`.vibin/migrations/0004-vibin-v2.md`.
