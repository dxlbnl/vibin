---
id: 9
title: Opt-in browser / UI verification (Playwright + Chrome DevTools MCP)
status: released
seed_commit: 87f0d2b
date: 2026-05-24
---

## Summary

The `reviewer` ran the test suite and checked the wiki, but "the suite is green" did not prove
"it works in a browser" for a frontend project. This adds **opt-in UI verification**: a spec
scenario whose `THEN` is a rendered outcome is tagged `Scenario (UI):`, and in a
**browser-enabled** project it's verified in a real browser — a committed **Playwright** test
(written by the `test-writer`, re-run in CI) plus a **Chrome DevTools MCP** drive-and-inspect
at review (golden path, console/network/a11y, screenshot evidence). It's **folded into the
existing tests-first + reviewer flow** (no new stage), backed by a new `browser-testing`
practice, and **enabled per project by `/bootstrap`** for frontend stacks — dormant otherwise.
Implements proposal 0005 (the last one).

## Detect (is this migration needed?)

Needed if **either** is true:

- `.claude/practices/browser-testing.md` does **not** exist; or
- `.claude/agents/reviewer.md` has **no** "Browser verification" section.

If the practice exists and the reviewer has the browser-verification section, the project is
already at this version — skip.

## Apply — seed-owned tooling

Child-machinery (adopt the latest seed text wholesale if unmodified locally; reconcile by hand
if customized). `git show 87f0d2b -- <path>` shows the exact change when the seed is available.

- `.claude/practices/browser-testing.md` — **new**; adopt the seed copy. Add the
  `browser-testing` row to `.claude/practices/README.md`'s mapping table.
- `wiki/specs/README.md` — the `Scenario (UI):` convention; `.claude/agents/spec-writer.md`
  emits it for rendered-UI outcomes.
- `.claude/agents/test-writer.md` — writes a Playwright test for each UI scenario when browser
  testing is enabled (assert by role/text), else falls back and reports the gap.
- `.claude/agents/reviewer.md` — the **"Browser verification (UI items, when enabled)"**
  section (launch the app, drive via Chrome DevTools MCP, check console/network/a11y, attach a
  screenshot, FAIL back to the implementer).
- `.claude/skills/manager/SKILL.md` — the `browser-testing` mapping row + the **UI
  verification** dispatch note.
- `.claude/skills/bootstrap/SKILL.md` — frontend → enable (Playwright dep + config + permission
  entry + `architecture.md` note + Rule; describe the Chrome DevTools MCP setup for the user);
  non-UI → prune the practice.
- `CLAUDE.md` — the UI-verification operational rule.

## Apply — project content (content-aware)

Capability is **off until enabled**. A **frontend** project that wants UI verification follows
bootstrap's enable steps (scaffold Playwright + permission entry; record the browser test
command, run command, and "browser testing: enabled" in `wiki/architecture.md` with a Rule;
apply the described Chrome DevTools MCP setup). A **non-UI** project removes
`.claude/practices/browser-testing.md` and changes nothing else.

## Verify

Confirm with read-only tools (`Grep`/`Read`/`Glob`) — no Bash, no prompts:

- `.claude/practices/browser-testing.md` exists with `## Knowledge`, `## Rationalizations →
  rebuttals`, `## Red flags`, `## Verification`; it has a row in the practices README mapping.
- `wiki/specs/README.md` defines `Scenario (UI):`; `.claude/agents/spec-writer.md` emits it.
- `.claude/agents/test-writer.md` writes Playwright tests for UI scenarios when enabled;
  `.claude/agents/reviewer.md` has a "Browser verification" section using the Chrome DevTools
  MCP + screenshot evidence.
- `.claude/skills/manager/SKILL.md` maps + dispatches UI verification;
  `.claude/skills/bootstrap/SKILL.md` has the frontend-enable + non-UI-prune steps; `CLAUDE.md`
  carries the UI-verification rule.
