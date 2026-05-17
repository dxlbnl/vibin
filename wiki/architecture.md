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

## Key technical decisions

Summarize here; the full rationale log is in `decisions.md`.

- *Example: Use Hono over Express for typed routing and edge compatibility.*
- *Example: Co-locate tests with source for fast navigation.*
