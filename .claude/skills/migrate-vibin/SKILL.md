---
name: migrate-vibin
description: Bring an existing Vibin project up to date with the latest seed by diffing the project's recorded seed commit against the latest Vibin on GitHub and applying the changes. Use when the user says "migrate", "migrate-vibin", "upgrade Vibin", "apply the latest seed changes", or after a new seed release. Reads the seed commit hash from .vibin-version, audits the project's actual state, applies the migrations added since, and reconciles seed-owned files while preserving local customizations.
disable-model-invocation: false
---

# Migrate a Vibin project to the latest seed

Vibin is cloned per project, so seed improvements (its `.claude/**` agents, skills, hooks,
`CLAUDE.md`, `README.md`, and wiki templates) do not arrive automatically. This skill brings
a project up to date by **diffing the seed commit the project was last synced to against the
latest Vibin on GitHub**, then applying the changes — without clobbering local
customizations.

## STEP 0 — read the wiki (mandatory, enforced)

**Read `wiki/INDEX.md` first.** A `PreToolUse` hook blocks writes and Bash until you do. The
migration touches the wiki, so you must be working from the current source of truth.

## How versioning works

- `.vibin-version` (repo root) holds the **git commit hash** of the Vibin seed this project
  is currently synced to. `/bootstrap` stamps it with the seed commit the project was cloned
  from; this skill updates it after a successful upgrade. **The Vibin seed repo itself ships
  no `.vibin-version`** — it is its own latest, so the marker is meaningless there and
  exists only in downstream clones.
- The Vibin seed lives on GitHub at **`dxlbnl/vibin`** (use the `mcp__github__*` tools, e.g.
  `get_commit`, `get_file_contents`, `list_commits`). If those tools are not available in
  this project, add the seed as a git remote and `git fetch` instead, then diff locally.
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
Get the list of seed-owned files changed between BASE and LATEST on GitHub — anything under
`.claude/**`, plus `CLAUDE.md`, `README.md`, `CHANGELOG.md`, `migrations/**`, and the
`wiki/` **template** pages as they ship in the seed. This is the set of upstream changes to
consider.

### 3. Preflight audit — check the project's actual state (don't worry about every file; DO check the important ones)
For each changed seed-owned file, classify the project's local copy by comparing it against
the seed's copy **at BASE** (fetch BASE's version from GitHub):

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
- **Seed-owned files**: adopt LATEST (unchanged-locally) or 3-way reconcile (customized),
  per the audit.
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
- **Audit before applying.** Always classify a file (unchanged vs. customized) before
  touching it — never blind-overwrite a seed-owned file the project may have customized.
- **Important files first.** Agents, skills, hooks, and `CLAUDE.md` are the ones that must be
  correct; don't block the whole migration over cosmetic doc drift.
- **Respect `decisions.md` append-only** — header/format-template text may be updated, past
  entry bodies may not.
- **No ad-hoc interpreters.** Apply edits with `Read`/`Edit`/`Write`, moves with `git mv`;
  do not write throwaway `node`/`python` scripts to transform files (see CLAUDE.md). Reading
  the GitHub diff is done with the `mcp__github__*` tools or a `git fetch` + `git diff`.
- **This seed repo is its own latest**, so running `/migrate-vibin` here is a no-op (and the
  seed carries no `.vibin-version`). The skill is for downstream clones.
