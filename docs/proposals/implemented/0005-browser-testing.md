# Proposal 0005 — Browser testing / UI verification

- **Status:** Implemented — ships as migration `0009` (`migrations/0009-browser-testing.md`)
- **Date:** 2026-05-22
- **Affects:** the `reviewer` stage / a new specialist, `/bootstrap`, project permissions
- **Ships as:** migration `0009`

## Context

Vibin's `reviewer` runs the test suite and checks the wiki — but for a frontend project,
"the suite is green" does not prove "the feature actually works in a browser." There is no
UI-verification story today, even though good practice (and CLAUDE.md's own default) is to
exercise UI changes in a real browser. Addy's `browser-testing-with-devtools` skill uses the
Chrome DevTools MCP to drive and observe the running app; Vibin should have an equivalent,
opt-in for projects that have a UI.

## Goal / non-goals

**Goal.** For items with UI acceptance criteria, back "tests pass" with "the golden path and
key states actually render/behave in a browser," and capture evidence (e.g. a screenshot).

**Non-goals.** Forcing browser verification on non-UI projects (it ships dormant in the
stack-agnostic seed). Throwaway automation — verification goes through a proper tool/MCP or a
committed browser test, never ad-hoc `node`/`python` scripts (per CLAUDE.md).

## Design

- **A `browser-tester` capability** — either a dedicated specialist agent or an extension of
  the `reviewer` for UI items. It launches the running app and drives it via a browser
  automation tool (Chrome DevTools MCP, or Playwright committed into the suite), checks the
  golden path + key states named in the spec, and attaches evidence.
- **Pipeline wiring.** For a `feature`/`bug` item whose spec has UI criteria, after the unit
  suite is green the reviewer (or the specialist) runs browser verification and reports it as
  part of PASS/FAIL. A failure here is a normal review finding routed back to the
  implementer.
- **Project-scoped, not seed-wide.** `/bootstrap` detects a frontend stack and enables it:
  writes the specialist agent, adds the Chrome DevTools MCP (or Playwright) to the project's
  permissions/config, and records the convention. The stack-agnostic seed ships it off.
- **Evidence.** Save a screenshot / trace artifact per verified item so "it works" is
  reviewable, not just asserted.

## Open questions

- Chrome DevTools MCP vs. committed Playwright tests as the default mechanism — MCP is
  lighter to drive interactively; Playwright lives in the suite and re-runs in CI. Possibly
  offer both (MCP for exploratory reviewer checks, Playwright for regression coverage).
- Whether browser verification is a separate pipeline stage or folded into `reviewer`.
- How a spec marks an item as "has UI to verify" (a criterion convention, or a card flag).

## Files the implementation will touch

- `.claude/agents/` — a `browser-tester` specialist (or a `reviewer` UI section), likely
  written by `/bootstrap` for frontend projects.
- `.claude/skills/manager/SKILL.md` — dispatch browser verification for UI items.
- `.claude/skills/bootstrap/SKILL.md` — detect a frontend stack → enable the capability,
  add the MCP/permissions.
- `.claude/settings.json` (per project, via bootstrap) — the browser-automation tool/MCP
  permissions.
- `CLAUDE.md` — note that UI verification uses a proper tool/committed test, not ad-hoc
  scripts.

## Sources

- Addy Osmani `agent-skills` — https://github.com/addyosmani/agent-skills
  (`browser-testing-with-devtools`, Chrome DevTools MCP).
