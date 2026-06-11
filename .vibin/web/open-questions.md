---
title: Open questions (unresolved forks)
type: question
status: draft
---

Live forks the wiki hasn't settled — for you to weigh in on:

- **Capture mechanism** ([capture](capture.md)) — hook-harvested (observe → candidates) vs a forced cheap step
  at close? Automatic is the bet, but how automatic?
- **Native memory vs wiki atoms** — reuse Claude Code's `~/.claude/.../memory/` format, a wiki-resident atom
  format, or both ([the atom](the-atom.md))? They are nearly the same unit.
- **Confidence** — is it worth computing for [Vibin knowledge](the-atom.md) in practice, or does it become a
  number nobody updates?
- **Strangle vs rebuild** — evolve old Vibin in place, or stand v2 up beside it and migrate? (Lean: strangle.)
- **Proving ground** — re-run a slice of `zod4-mock` under v2 to test whether the wiki would have caught the
  override saga ([self-improvement](self-improvement.md))?

**Why:** naming the forks keeps them visible instead of silently decided. Each becomes a [decision](the-atom.md)
once settled.
