# Vibin v2 — build plan

How we go from the design wiki ([web/](web/index.md)) to a working, test-driven Vibin v2.

> **Status (2026-06-11):** Phases 1–3 are **done in the visuals (Fosfor) testbed** — note: the proving
> ground became `visuals`, not `zod4-mock`, and the toy phase was skipped for a real migration (the
> right call in hindsight: compounding only shows on a dense, real graph). Knowledge graph (21 atoms),
> all hooks (knowledge gate / capture / retrieval suggester / session injection), v2 skills
> (loop / intake / status), reviewer + researcher agents, sprint file, swept backlog. **First real task
> (tap-tempo smoothing) ran the full loop**: retrieve → fix → atom updated in place → card deleted.
> Phase 4 (more live tasks, e.g. B11) is next. **The seed backport has landed**: seed-meta
> consolidated under `.vibin/`, `wiki/` rebuilt as the v2 template, hooks/skills/agents/CLAUDE/README
> replaced with the v2 set (generalized from visuals), `migrate-vibin` repointed, and migration
> `0004-vibin-v2.md` documents the v1 → v2 path for downstream projects.

**Principle:** prove the riskiest assumption cheapest first; dogfood; **strangle v1, don't rebuild**.
[web/from-wiki-to-machinery.md](web/from-wiki-to-machinery.md) says what each piece becomes (hook / skill /
agent / config). The keystone is **capture + retrieval** ([web/capture.md](web/capture.md)) — if knowledge
doesn't compound cheaply, the whole model is wrong, so we test that first.

## Phase 1 — The keystone, proven on a toy *(highest risk first)*
Goal: show that knowledge compounds — capture → retrieve → better next task — or learn early that it doesn't.
- Pin the **atom format** (frontmatter + file conventions) — extends [web/the-atom.md](web/the-atom.md).
- Build **capture**: a hook that records candidate learnings at task close + a distil step that writes atoms.
- Build **retrieval**: read-before-work (extend the existing wiki-gate hook) + how atoms are surfaced.
- Prove on ONE real task: do it → auto-capture 1 atom → next task retrieves it → visibly better.
- **Decision needed:** how automatic is capture (hook vs forced step)? — [web/open-questions.md](web/open-questions.md).
- **GATE:** if the loop doesn't spin on a toy, stop and rethink before building more.

## Phase 2 — The lightweight track + gates
- **Light-track skill**: acceptance criteria + tests, no separate spec (test-is-the-spec, [web/right-sizing.md](web/right-sizing.md)).
- **Gates as hooks**: read-before-work, capture-before-close, green-means-green ([web/gates.md](web/gates.md)).
- **Working-state files**: `sprint.md` (tasks + run log) + `backlog/` of work-item nodes ([web/the-backlog.md](web/the-backlog.md)).

## Phase 3 — Migrate a real project (`zod4-mock`)
- Build the **bootstrap / migrate skill** (ingest v1 `wiki/` + `decisions.md` + specs + code → starter graph).
  Port the `karpathy-llm-wiki` skill ([web/bootstrapping-the-web.md](web/bootstrapping-the-web.md)).
- Run it on `zod4-mock`: distil durable atoms, **keep the binding Rules verbatim**, turn the 14 inbox items into
  work-item cards. Output: a small, linked v2 graph + backlog.

## Phase 4 — Test-drive on a real task
- Pick a real `zod4-mock` item (a small feature, or a bug).
- Run the full loop: pull into sprint → retrieve → do (light track) → verify → capture → close → [retro](web/the-retro.md).
- **Measure**: did retrieval help? did capture yield a reused atom? lighter than v1?
- **Stress test**: would the retro / trip-wire have caught the override saga (B12→B53→B134→B136)?

## Phase 5 — Agents + skill library
- Author the **few** agents (cold-spawn cost — keep them few): likely implementer, reviewer, researcher.
- Port the **skill library** ([web/onboarding-agents-and-skills.md](web/onboarding-agents-and-skills.md)):
  intent-driven, knowledge-ops, deep-research, verification, context-budget, retro, gh-issue-intake, bootstrap.
- **Config**: stack permission profile + model routing ([web/cost-and-models.md](web/cost-and-models.md)).

## Phase 6 — Self-improvement + grand pillars *(only after the loop spins)*
- Trip-wire hook + retro-driven proposals, human-reviewed ([web/self-improvement.md](web/self-improvement.md)).
- Cheap-trust artifact, pass@k where it matters, safe-autonomy + security
  ([web/cheap-trust.md](web/cheap-trust.md), [web/safe-autonomy.md](web/safe-autonomy.md)).

## Throughout
- **Strangle, not rebuild** — keep v1's Rules + test-first; replace the heavy relay incrementally.
- **Dogfood** — build v2 using v2 as soon as Phase 2 stands.
- **Measure reuse** — retrieval/reuse rate climbing = the loop works; a growing-but-unread wiki = kill signal.

## Immediate next 2 actions
1. **Decide the capture mechanism** (the keystone fork) — hook-harvested vs forced step at close.
2. **Start Phase 1** — pin the atom format, write the capture + retrieval hooks, prove on one toy task.
