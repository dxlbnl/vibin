# Architecture

> **Starter template.** `/bootstrap` fills this in from your answers. Edit freely.
> This page drives the stack-agnostic scaffolding and is the source of truth for the
> package manager / test runner / language toolchain that agents will use.

## Tech stack

- **Language**: *Example: TypeScript 5.x*
- **Runtime / platform**: *Example: Node 20 LTS, runs in the browser via Vite*
- **Key frameworks / libraries**: *Example: React, Tailwind, Hono on the backend*

## Package manager (binding)

> Agents must use **only** this package manager. Do not substitute another even if
> tutorials, generated configs, or model priors suggest one. If the project needs a
> different one, change it here first and re-bootstrap.

- **Package manager**: *Example: pnpm (use only this — not npm, not yarn)*

## Test setup

- **Test runner**: *Example: vitest*
- **Test command**: *the exact command to run the full suite — Example: `pnpm test`*
- **Test file location / naming**: *Example: co-located `*.test.ts` next to source*

> `/bootstrap` records the stack-specific package manager / runtime / test runner in
> `.claude/settings.json` `permissions.allow` so the agents can run them without a
> prompt each time. The seed itself ships only stack-agnostic permissions.

## Project structure

How the codebase is laid out at the repo root (single project per clone — lives at
the root).

```
src/        # application code
tests/      # if not co-located
```

*Example:*

```
src/
  components/   # React UI
  server/       # Hono routes
  lib/          # shared utilities
```

## Rules (binding)

Standing constraints all future work MUST follow. This is the **propagation channel**:
agents read this page before writing code, so a convention only sticks if it lands here.
Each rule is one line, derived from a decision in `decisions.md`, and cites it (→ D<n>).
Keywords follow [RFC 2119](https://datatracker.ietf.org/doc/html/rfc2119) — **MUST**,
**MUST NOT**, **SHOULD**, **MAY** — reserve MUST for genuinely binding rules so the words
keep their weight. The **manager owns this list**: it promotes a reviewer-flagged
standing constraint into a rule when an item is done.

**Keep this list short.** It is an index of constraints, not the rationale — the "why"
lives in `decisions.md` via the link, and a bloated list erodes adherence. A rule may
carry an optional `applies:` scope when it is not universal.

- *Example: All UI components **MUST** come from `@acme/ui`; do not hand-roll styled elements. (→ D7)*
- *Example: API handlers **SHOULD** validate input at the boundary with `zod`. applies: `src/server/**` (→ D5)*
- *Example: Dates **MAY** use `date-fns`; no other date library. (→ D9)*
