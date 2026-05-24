---
id: 10
title: Search with the Grep/Glob/Read tools, not shelled Bash
status: released
seed_commit: c949d1a
date: 2026-05-24
---

## Summary

Pipeline agents were shelling out `grep`/`find`/`rg` for code search, which forces a
permission prompt every time (a shelled search rarely matches the project's Bash allowlist),
while the internal `Grep`/`Glob`/`Read` tools are auto-allowed and silent. This makes
tool-first search a rule: a new CLAUDE.md operational rule (sibling to the no-ad-hoc-
`node`/`python` rule) plus a concise "Searching: use the tools, not Bash" section in the
Bash-capable agents (`reviewer`, `test-writer`, `implementer`). `spec-writer` has no Bash tool,
so it is unaffected. Bash stays for the project's own commands (test runner, `git`), run one
per call — chaining with `;`/`&&` also defeats the allowlist and prompts.

## Detect (is this migration needed?)

Needed if **either** is true:

- `CLAUDE.md` has no "Search with tools, not Bash" operational rule; or
- `.claude/agents/reviewer.md` has no "Searching: use the tools, not Bash" section.

If CLAUDE.md carries the rule and the Bash-capable agents have the section, the project is
already at this version — skip.

## Apply — seed-owned tooling

Child-machinery (adopt the latest seed text wholesale if unmodified locally; reconcile by hand
if customized). `git show c949d1a -- <path>` shows the exact change when the seed is available.

- `CLAUDE.md` — the **"Search with tools, not Bash"** operational-rules bullet.
- `.claude/agents/reviewer.md`, `.claude/agents/test-writer.md`,
  `.claude/agents/implementer.md` — a `## Searching: use the tools, not Bash` section after
  STEP 0. (Do **not** add it to `spec-writer.md` — it has no Bash tool.) If the project wrote
  Bash-capable specialist agents at bootstrap, add the same section to them.

## Apply — project content (content-aware)

None. This migration changes only seed tooling; it does not touch project wiki content.

## Verify

Confirm with read-only tools (`Grep`/`Read`/`Glob`) — no Bash, no prompts:

- `CLAUDE.md` contains a "Search with tools, not Bash" rule pointing at `Grep`/`Glob`/`Read`.
- `.claude/agents/reviewer.md`, `test-writer.md`, and `implementer.md` each contain a
  "Searching: use the tools, not Bash" section.
- `.claude/agents/spec-writer.md` does **not** (it has no Bash tool).
