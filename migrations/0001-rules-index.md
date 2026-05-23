---
id: 1
title: Standing decisions propagate via an architecture.md Rules index
status: released
seed_commit: 44b772a
date: 2026-05-22
---

## Summary

`wiki/decisions.md` was effectively **write-only** — no pipeline agent read it before
generating code — so standing conventions (e.g. "use the component library, don't
hand-roll UI") failed to reach the next session, and the log accumulated noise. Binding
conventions now live as a short, **RFC-2119 `## Rules` index** inside `wiki/architecture.md`
(the page agents already read at STEP 0). `decisions.md` becomes the rationale archive,
scoped to **standing constraints** only. The **manager** owns the Rules index: subagents
write the rationale and flag the constraint, the reviewer confirms a decision exists, and
the manager promotes it to a one-line rule when an item is done.

## Detect (is this migration needed?)

Needed if **either** is true:

- `wiki/architecture.md` has **no** `## Rules (binding)` section (likely still has a
  `## Key technical decisions` heading); or
- the phrase "notable design/tech choice" still appears in `.claude/**` or `CLAUDE.md`.

If `wiki/architecture.md` already has `## Rules (binding)` and no "notable choice" wording
remains, the project is already at v1 — skip.

## Apply — seed-owned tooling

Adopt the latest seed text for each file below (wholesale if unmodified locally; by hand if
customised). When the upstream seed is available, `git show 44b772a -- <path>` shows the
exact change.

- `.claude/agents/reviewer.md` — new verification check: detect & flag whether the item
  established/changed a standing constraint; confirm a backing `decisions.md` entry exists
  (the reviewer flags, it does not edit the wiki).
- `.claude/agents/spec-writer.md` — new "Spec freshness" section (every criterion must
  comply with the current Rules); the decisions bullet rescoped to standing constraints.
- `.claude/agents/implementer.md` — new "follow `architecture.md`'s Rules" bullet; decisions
  bullet rescoped; one-off choices go to `progress.md`.
- `.claude/agents/test-writer.md` — `MUST`/`MUST NOT` rules are part of the contract and are
  tested as written; decisions bullet rescoped.
- `.claude/skills/manager/SKILL.md` — the manager **owns the Rules index**: the **mandatory
  promote-to-rule step** in the per-item "Done" checklist, Rules-compliance in the
  spec-writer dispatch template, and the Rules section in its bookkeeping/logging scope.
- `.claude/skills/wiki/SKILL.md` — "Decisions & rules" convention; the `architecture.md`
  description now includes the Rules index.
- `.claude/skills/wiki-sync/SKILL.md` — detects **rule drift** (a rule whose decision is
  missing/superseded, a standing-constraint decision with no rule, or code violating a rule).
- `.claude/skills/bootstrap/SKILL.md` — seeds the Rules section and links `D1`/`D2` to their
  rules. *Affects future bootstraps only — no action for existing projects beyond adopting
  the text.*
- `CLAUDE.md` — the "Decision ownership" bullet is replaced by a "Decisions and rules"
  bullet.
- `README.md` — new "Decisions and rules" section.

## Apply — project content (content-aware)

1. **`wiki/architecture.md`**
   - Rename the `## Key technical decisions` section to `## Rules (binding)` and add the
     preamble from the seed template (RFC-2119 keywords; "the manager owns this list";
     "keep this list short"; optional `applies:` scope).
   - **Backfill the rules.** Read every entry in `wiki/decisions.md`. For each decision that
     is a *standing constraint* (a dependency/tool to use or ban, a pattern code must
     follow, an architectural boundary), write **one** RFC-2119 line citing it — e.g.
     `- Components **MUST** come from \`@acme/ui\`. (→ D7)`. Skip local or one-off decisions.
2. **`wiki/decisions.md`**
   - Replace the header blockquote with the new "What belongs here" standing-constraint bar
     (copy from the seed template).
   - Add the `Rule added/changed` field to the **Format** block.
   - Do **not** edit existing entries' bodies (append-only); new entries use the new format
     going forward.
3. **`wiki/INDEX.md`**
   - Update the `architecture.md` row in the Pages table to mention the **Rules** index.
   - Replace the "Decisions" convention bullet with the "Decisions & rules" bullet (copy
     from the seed template).

## Verify

- `wiki/architecture.md` has a `## Rules (binding)` section, and every standing-constraint
  decision in `wiki/decisions.md` has a matching rule citing its `D<n>`.
- Run `/wiki-sync` — it reports no rule/decision drift (or flags exactly the gaps you
  expect).
- No lingering `## Key technical decisions` heading and no "notable design/tech choice"
  phrasing remain in `.claude/**` or `CLAUDE.md`.
