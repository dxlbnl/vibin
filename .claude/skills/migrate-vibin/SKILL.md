---
name: migrate-vibin
description: Bring an existing Vibin project up to date with the latest seed by diffing the project's recorded seed commit against the latest Vibin on GitHub and applying the changes. Use when the user says "migrate", "migrate-vibin", "upgrade Vibin", "apply the latest seed changes", or after a new seed release. Reads the seed commit hash from .vibin-version, audits the project's actual state, applies the migrations added since, and reconciles seed-owned files while preserving local customizations.
disable-model-invocation: false
---

# Migrate a Vibin project to the latest seed

Vibin is cloned per project, so seed improvements (its `.claude/**` agents, skills, hooks,
`CLAUDE.md`, and wiki **template** pages) do not arrive automatically. This skill brings
a project up to date by **diffing the seed commit the project was last synced to against the
latest Vibin on GitHub**, then applying the changes — without clobbering local
customizations.

## What propagates to a project — and what never does

The seed repo contains two kinds of files. Classify every changed file before touching it:

- **Child machinery (propagates — adopt or reconcile):** everything under `.claude/**`,
  `CLAUDE.md`, and the seed-owned **wiki template / README** pages (e.g.
  `wiki/specs/README.md`, `wiki/backlog/README.md`). These are the pipeline itself, so a
  project needs the latest version.
- **Seed-meta (NEVER written into a project):** `migrations/**`, `CHANGELOG.md`, `docs/**`,
  and the repo-root `README.md`. These document Vibin's own evolution; a project needs only
  the *effects* of a migration, not the migration files or Vibin's changelog/proposals. The
  skill **reads** `migrations/NNNN-*.md` from GitHub to learn each migration's content-aware
  steps and **applies** those steps, but it **never creates** these files in the project.
  (`.vibin-version` is the one marker a project keeps — this skill updates it.)

## STEP 0 — read the wiki (mandatory, enforced)

**Read `wiki/INDEX.md` first.** A `PreToolUse` hook blocks writes and Bash until you do. The
migration touches the wiki, so you must be working from the current source of truth.

## How versioning works

- `.vibin-version` (repo root) holds the **git commit hash** of the Vibin seed this project
  is currently synced to. `/bootstrap` stamps it with the seed commit the project was cloned
  from; this skill updates it after a successful upgrade. **The Vibin seed repo itself ships
  no `.vibin-version`** — it is its own latest, so the marker is meaningless there and
  exists only in downstream clones.
- The Vibin seed lives on GitHub at **`dxlbnl/vibin`**, and the whole migration runs off its
  **HTTP API** — not local git. (Projects are often cloned with `.git` wiped before
  `/bootstrap`, so there is no shared history to `git diff` against; the API only needs the
  two commit hashes.) Use the `mcp__github__*` tools where available
  (`get_commit`, `get_file_contents`, `list_commits`), or an authenticated HTTPS GET to
  `api.github.com`. The key call is the **compare** endpoint:
  `GET /repos/dxlbnl/vibin/compare/<BASE>...<LATEST>`, which returns every changed file with
  its status and patch in one response.
- **The diff itself tells you what to run.** Which migrations apply = the
  `migrations/NNNN-*.md` files that are *newly added* between BASE and LATEST. There is no
  version-number arithmetic — read those new migration files and follow their content-aware
  steps (e.g. 0001 says to triage `wiki/decisions.md` and promote the standing constraints
  into `architecture.md`'s Rules section).

## Procedure

### 1. Establish the two endpoints
- **BASE** = the hash in `.vibin-version`. If the file is missing, this project predates the
  marker — stop and ask the user for the Vibin commit it was cloned from (or the approximate
  date), then proceed with that as BASE.
- **LATEST** = the head commit of `dxlbnl/vibin`'s default branch (via GitHub).
- If `BASE == LATEST`, report "already up to date" with the hash and stop.

### 2. Compute the seed diff (BASE → LATEST)
Call the GitHub **compare** API (`GET /repos/dxlbnl/vibin/compare/<BASE>...<LATEST>`) to get
every changed file with its patch — this is the set of upstream changes to consider, and it
needs no local git history. Split the changed entries using the classification above:
**child machinery** (under `.claude/**`, plus `CLAUDE.md` and the `wiki/` template/README
pages) is what you adopt or reconcile into the project. **Seed-meta** (`migrations/**`,
`CHANGELOG.md`, `docs/**`, the repo-root `README.md`) is **not** written to the project — but
note which `migrations/NNNN-*.md` files are **newly added**, because their content-aware
steps drive the project-content updates in step 4.

### 3. Preflight audit — check the project's actual state (don't worry about every file; DO check the important ones)
For each changed **child-machinery** file (skip seed-meta — it is never written here),
classify the project's local copy by comparing it against the seed's copy **at BASE** (fetch
it with `get_file_contents` at `ref=<BASE>`, or the API's raw content URL):

- **Unchanged locally** (project copy == seed@BASE) → safe to adopt the LATEST version
  wholesale.
- **Customized locally** (project copy != seed@BASE) → the project diverged. Do **not**
  overwrite. Apply the BASE→LATEST change as a 3-way reconcile by hand, preserving the local
  customization, and call it out in the report.

Prioritize the **load-bearing files** in this audit — the agent definitions
(`.claude/agents/**`), the skills (`.claude/skills/**`), the hooks (`.claude/hooks/**`), and
`CLAUDE.md`. A drifted agent or hook is what actually breaks the pipeline; trivial doc drift
is not worth fussing over.

### 4. Apply
- **Child machinery**: adopt LATEST (unchanged-locally) or 3-way reconcile (customized),
  per the audit.
- **Seed-meta** (`migrations/**`, `CHANGELOG.md`, `docs/**`, repo-root `README.md`): do
  **not** write these into the project. Read the newly-added `migrations/NNNN-*.md` from
  GitHub for their steps only — do not create a `migrations/` dir, a `CHANGELOG.md`, a
  `docs/` tree, or overwrite the project's own `README.md`.
- **Project content** (`wiki/*.md` that hold *project-specific* content, not templates):
  never derive these from a raw diff. For each `migrations/NNNN-*.md` file that the diff
  shows as newly added, follow its **content-aware** steps to adapt the project's own wiki
  content to the new structure. **Never rewrite the body of a past `wiki/decisions.md`
  entry** (append-only).

### 5. Record and commit
- Write **LATEST** to `.vibin-version`.
- Stage the changed files and `.vibin-version`; commit `chore: migrate Vibin seed to <short-hash>`.
- Do not push unless the user asks.

### 6. Report
State: BASE → LATEST hashes, files adopted wholesale, files reconciled by hand (and how),
any content-aware wiki steps applied, and anything that needs the user's attention.

## Rules
- **Never write seed-meta into the project.** `migrations/**`, `CHANGELOG.md`, `docs/**`,
  and the repo-root `README.md` are Vibin's own evolution log — a project gets the *effects*
  of a migration, never the files. Read migrations from GitHub for their steps; do not copy
  them, the changelog, the proposals, or Vibin's README into the project.
- **Audit before applying.** Always classify a file (unchanged vs. customized) before
  touching it — never blind-overwrite a child-machinery file the project may have customized.
- **Important files first.** Agents, skills, hooks, and `CLAUDE.md` are the ones that must be
  correct; don't block the whole migration over cosmetic doc drift.
- **Respect `decisions.md` append-only** — header/format-template text may be updated, past
  entry bodies may not.
- **API, not local git.** The diff comes from the GitHub compare API (the project may have no
  `.git` history back to the seed). Apply edits with `Read`/`Edit`/`Write`; do not write
  throwaway `node`/`python` scripts to transform files (see CLAUDE.md).
- **This seed repo is its own latest**, so running `/migrate-vibin` here is a no-op (and the
  seed carries no `.vibin-version`). The skill is for downstream clones.
