# Changelog

This file records changes to the Vibin **seed** — its agents, skills, hooks, operating
rules (`CLAUDE.md`), and wiki **templates**. Vibin is cloned per project, so seed
improvements do **not** reach already-bootstrapped projects automatically. Each entry
below therefore carries a **Migration** section an agent can execute against an existing
repo to bring it up to date.

## How to use this changelog (for agents)

When asked to "upgrade this project to the latest Vibin" (or similar):

1. Read this file newest-first and find every entry whose changes are **not yet present**
   in this project. There is no version marker file — infer from the repo itself (e.g.
   "does `wiki/architecture.md` already have a `## Rules (binding)` section?").
2. Apply the **Migration** steps of each missing entry **in order, oldest first**.
3. Two kinds of file need different handling:
   - **Seed-owned tooling** — `.claude/**`, `CLAUDE.md`, `README.md`. These are not
     project-specific. If a file is unmodified locally, adopt the new seed version
     wholesale; if it was locally customised, apply the described edits by hand. When you
     have the upstream seed available, `git show <seed-commit> -- <path>` (or a diff
     against the seed) shows the exact change.
   - **Project wiki content** — `wiki/*.md`. These hold project-specific content. **Never
     blind-overwrite.** Follow the content-aware steps, which adapt the project's own
     content to the new structure.
4. Respect `wiki/decisions.md`'s **append-only** rule: never rewrite the body of a past
   decision entry during a migration. Header/format-template text is fine to update.
5. Commit the migration as its own commit, e.g. `chore: upgrade Vibin seed — <entry title>`.

---

## 2026-05-22 — Standing decisions propagate via an `architecture.md` Rules index

> Seed commit (this repo): `44b772a`.

### Why

`wiki/decisions.md` was effectively **write-only** — no pipeline agent read it before
generating code — so standing conventions (e.g. "use the component library, don't
hand-roll UI") failed to reach the next session, and the log accumulated noise. Binding
conventions now live as a short, RFC-2119 **Rules** index inside `wiki/architecture.md`
(the page agents already read at STEP 0). `decisions.md` becomes the rationale archive,
scoped to **standing constraints** only. The **manager** owns the Rules index.

### Seed-owned tooling — adopt the new text

- `.claude/agents/reviewer.md` — new verification check: detect & flag whether the item
  established/changed a standing constraint; confirm a backing `decisions.md` entry
  exists (reviewer flags, does not edit the wiki).
- `.claude/agents/spec-writer.md` — new "Spec freshness" section (every criterion must
  comply with the current Rules); the decisions bullet is rescoped to standing
  constraints.
- `.claude/agents/implementer.md` — new "follow `architecture.md`'s Rules" bullet; the
  decisions bullet is rescoped; one-off choices go to `progress.md`.
- `.claude/agents/test-writer.md` — MUST/MUST NOT rules are part of the contract and are
  tested as written; the decisions bullet is rescoped.
- `.claude/skills/manager/SKILL.md` — the manager **owns the Rules index**: adds the
  **mandatory promote-to-rule step** in the per-item "Done" checklist, Rules-compliance
  in the spec-writer dispatch template, and the Rules section to its bookkeeping/logging
  scope.
- `.claude/skills/wiki/SKILL.md` — "Decisions & rules" convention; the `architecture.md`
  description now includes the Rules index.
- `.claude/skills/wiki-sync/SKILL.md` — detects **rule drift** (a rule whose decision is
  missing/superseded, a standing-constraint decision with no rule, or code violating a
  rule).
- `.claude/skills/bootstrap/SKILL.md` — seeds the Rules section and links `D1`/`D2` to
  their rules. *Affects future bootstraps only — no action for existing projects beyond
  adopting the text.*
- `CLAUDE.md` — the "Decision ownership" bullet is replaced by a "Decisions and rules"
  bullet.
- `README.md` — new "Decisions and rules" section, plus this "Upgrading an existing
  project" note.

### Project wiki content — content-aware migration

1. **`wiki/architecture.md`**
   - Rename the `## Key technical decisions` section to `## Rules (binding)` and add the
     preamble from the seed template (RFC-2119 keywords; "the manager owns this list";
     "keep this list short"; optional `applies:` scope).
   - **Backfill the rules.** Read every entry in `wiki/decisions.md`. For each decision
     that is a *standing constraint* (a dependency/tool to use or ban, a pattern code must
     follow, an architectural boundary), write **one** RFC-2119 line citing it — e.g.
     `- Components **MUST** come from \`@acme/ui\`. (→ D7)`. Skip decisions that were
     local or one-off.
2. **`wiki/decisions.md`**
   - Replace the header blockquote with the new "What belongs here" standing-constraint
     bar (copy from the seed template).
   - Add the `Rule added/changed` field to the **Format** block.
   - Do **not** edit existing entries' bodies (append-only); new entries use the new
     format going forward.
3. **`wiki/INDEX.md`**
   - Update the `architecture.md` row in the Pages table to mention the **Rules** index.
   - Replace the "Decisions" convention bullet with the "Decisions & rules" bullet (copy
     from the seed template).

### Verify

- `wiki/architecture.md` has a `## Rules (binding)` section, and every standing-constraint
  decision in `wiki/decisions.md` has a matching rule citing its `D<n>`.
- Run `/wiki-sync` — it reports no rule/decision drift (or flags exactly the gaps you
  expect).
- No lingering `## Key technical decisions` heading and no "notable design/tech choice"
  phrasing remain in `.claude/**` or `CLAUDE.md`.
