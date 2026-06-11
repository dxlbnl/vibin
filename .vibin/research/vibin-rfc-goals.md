# RFC: What Vibin Should Be

**Status:** Draft / direction-setting. Goals, not implementation design.
**Inputs:** [[vibin-painpoints]] (the 11 pains) + a deep read of `affaan-m/ECC`
(212K★ agent system; mined for *mechanisms*, not its kitchen-sink scale).
**Requirements language:** MUST / MUST NOT / SHOULD / MAY per RFC 2119.
**One-line vision:** *A wiki-based, agentic, lean powerhouse* — the wiki is a compounding
**knowledge brain** of short cross-linked atoms, the **Rules** are its spine, and the process
is **proportional**: heavy only where the work is risky.

---

## 1. The core to build up from

Strip Vibin to what actually earned its keep and discard the rest:

- **KEEP** — the wiki-as-source-of-truth *idea*; the binding **Rules** index (RFC-2119
  standing constraints); test-first verification; resumability.
- **DROP/REBUILD** — the heavyweight per-item spec relay, the ever-growing monolithic
  journal/decisions logs, the 4-lane kanban that retains everything forever, the
  fixed-template ceremony applied regardless of risk.

The failure mode the pains describe is one sentence: **Vibin optimized for audit-trail
completeness over knowledge and leanness.** ECC optimizes the opposite and it's why ECC feels
like a powerhouse and Vibin feels like paperwork.

**Two-tier state (the load-bearing decision).** The **wiki is durable memory** — short atomic
learnings that MUST survive. The **run log is working scratch** — disposable, freely trimmable,
and discarded once its learnings are distilled into the wiki. Audit-trail completeness is *not*
a goal: the log exists to serve the current sprint, nothing more. The only thing that must
survive a sprint is what reached the wiki.

## 2. What ECC actually teaches (the transferable mechanisms)

| ECC mechanism | What it does | Vibin pain it answers |
|---|---|---|
| **Instincts** | atomic knowledge unit: `trigger` + `confidence (0.3→0.9)` + `Action` + `Evidence`; ~10 lines | P3, P4, P6 |
| **Continuous learning / `/evolve`** | knowledge *harvested from work*, confidence-weighted, clustered into skills, promoted at 2+ uses | P4 (authored books that get reversed) |
| **knowledge-ops** | "one canonical home per fact"; dedupe-before-store; cross-references required | P6, P11 |
| **intent-driven-development** | two depths, auto-chosen — *"use the smallest useful output"*; Quick Capture (3–7 criteria, no approval delay) vs Full Brief only when risky | P2, P3, P5 |
| **WORKING-CONTEXT** | a *rolling* log: only the current sprint is detailed; done work is summarized to archive | heavy backlog, P3 |
| **context-budget** | hard size budgets (rules <100 ln, skills <400, agents <200, CLAUDE.md <300) | P3 |
| **iterative retrieval** | handoff passes *objective/why*, not just the query; ≤3 follow-up cycles | cold artifact handoff |
| **pass@k / pass^k** | verification expresses *reliability*, not one green run | P7 |
| **shortform restraint** | *"fine-tuning, not architecture"*; parallelism only "out of true necessity" | the whole "lean" goal |

Note the convergence worth trusting: an ECC **instinct**, Claude Code's **native memory**
file, and a good **LLM-wiki page** are the *same atom* — short, trigger/confidence-tagged,
evidence-linked. Vibin's knowledge unit should be that atom too.

---

## 3. Goals

### A. The knowledge layer (make it a real wiki, not a kanban board)

- **G1 — Vibin MUST make the wiki a knowledge base of atomic, cross-linked pages**
  (one fact/page, short, `[[wikilinks]]` between them), distinct from work-tracking.
  *Today 75% of wiki files are work cards and the knowledge pages have zero cross-links (P6).*
- **G2 — Each knowledge page SHOULD carry an instinct-shaped header**: a `trigger` (when it
  applies), a `confidence`, and an `Evidence` link, so knowledge is retrievable and weightable.
- **G3 — Durable knowledge MUST compound from completed work**, harvested and promoted, rather
  than be hand-authored up front as heavyweight specs. *Reverses P4 (587-line spec, reversed in 48h).*
- **G4 — Every fact MUST have exactly one canonical home**; a contributor reconciles a
  duplicate, never adds a second copy. *Kills P11 (vision in 3 places).*
- **G5 — The binding Rules index MUST remain the spine** (it is the one artifact that works),
  but each rule MUST be a single genuine line that links to its rationale — not an 80–120-word
  paragraph. *Preserves the win in P-good while fixing P3.*

### B. Proportional process (lean by default, heavy only where it pays)

- **G6 — Vibin MUST size ceremony to risk.** The default path is lightweight (acceptance
  criteria + tests, no upfront spec page); the full spec/decision path is reserved for risky,
  architectural, or hard-to-reverse work. *Directly P2/P5.*
- **G7 — Vibin MUST offer a conversational track**: reason with the human toward acceptance
  criteria, instead of routing every report through a spec-writer that emits a book. *P5.*
- **G8 — Precision MUST live in executable tests; prose carries intent only** and MUST NOT
  restate what a test already asserts. *P4 (four copies of intent, none authoritative).*
- **G9 — Artifacts MUST obey size budgets** (e.g. spec ≤ ~1 page, rule = 1 line, run-log entry
  ≤ a few lines, `CLAUDE.md` ≤ 300 lines), enforced like ECC's `context-budget`. *P3.*

### C. Sprint-based working state; the log is disposable, the wiki is the keeper

- **G10 — Work MUST be organized as rolling sprints**, not an ever-accumulating backlog. The
  working state is the *current* sprint — its items plus a live scratch log; everything outside
  it is either durable knowledge (in the wiki) or gone. *You like the sprint idea; ECC's rolling WORKING-CONTEXT.*
- **G11 — The run log MAY be freely compressed, trimmed, or discarded** when a sprint closes —
  there is NO requirement to preserve a complete history. The single gate: a sprint MUST NOT be
  closed/trimmed until its durable learnings are captured in the wiki (G1–G3). *Trimming the
  1742-line journal / 1290-line log to nothing is fine, as long as the learnings reached the wiki.*

### D. Concurrency & git-flow (make branches usable)

- **G12 — Work state MUST be branch-mergeable by construction**: per-item fragment files (no
  shared append-only `progress.md`/`decisions.md`), and collision-free IDs (not a global
  `max+1`), so parallel branches don't conflict. *Directly P1.*
- **G13 — Parallelism MAY use git worktrees but SHOULD stay minimal** — "out of true
  necessity," not by default. *ECC restraint; avoids over-engineering.*

### E. Orchestration, handoff, verification

- **G14 — Subagent handoff MUST pass the objective ("why"), not just the task**, and SHOULD
  allow ≤3 iterative-retrieval follow-ups before an output is accepted. *ECC iterative retrieval; fixes cold one-shot handoff.*
- **G15 — Definition-of-done MUST be enforced**: an item MUST NOT be marked done with known-red
  tests; a persistent failure MUST become its own tracked item. *Fixes P7 (6 "done" items shipped red).*
- **G16 — Verification MAY express reliability, not just a single green run** (pass@k for
  "make it work", pass^k for must-not-regress paths). *ECC; optional, for critical surfaces.*

### F. Overarching

- **G17 — Skills SHOULD be the primary, composable surface**; tracks are composed from skills,
  not a single rigid 4-stage relay every item must traverse. *P2; ECC skills-first.*
- **G18 — Leanness is measured on the *always-on* footprint, not the library.** The standing
  context cost — **agent description blocks** (loaded into every Task context), **enabled MCP
  tools** (~500 tokens/schema), and **`CLAUDE.md`** (≤300 lines) — MUST stay small. A large,
  curated, *lazy-loaded* **skill library is NOT bloat** and is encouraged (G19). Agents MUST
  stay selective (one persisted only for a genuinely recurring, distinct role; descriptions
  ≤30 words); ad-hoc roles use `general-purpose`. *ECC's own context-budget; corrects the crude "few skills" reading.*
- **G19 — Vibin SHOULD cultivate a curated, well-authored skill library.** Port the best ECC
  *mechanisms* — `intent-driven-development`, `knowledge-ops`, `deep-research`/`search-first`,
  `verification-loop`, `context-budget` — **rewritten to Vibin's conventions, never bulk-copied**;
  skip ECC's domain/community noise. Each skill MUST earn its place and have one canonical home
  (G4); overlapping skills MUST be consolidated, not duplicated. *The win is a small opinionated
  core PLUS a deep on-demand toolbox — not a 200-skill always-on mega-plugin.*

---

## 4. Non-goals (what Vibin must NOT become)

- An *always-on* mega-plugin: dozens of agents loaded into every Task context, 80+ enabled MCP
  tools, cross-harness adapter sprawl, uncurated bulk-copied skills. A deep *lazy* skill library
  is welcome (G19) — the line is the always-on footprint and unported copy, not library size (G18).
- An audit-trail completist. Traceability is kept *only* where it changes a decision; it is no
  longer the primary design driver (§1).
- A second knowledge silo. The wiki, the Rules, and Claude Code's native memory MUST converge
  on one atom and one canonical home (G1/G2/G4), not multiply stores like ECC's six layers.

## 5. Open questions for the next pass (implementation design, not this doc)

- Concrete shape of the knowledge atom (frontmatter fields; how `confidence`/`trigger` are set
  and updated; manual vs hook-harvested).
- The exact lightweight default track (what replaces spec→test→impl→review for low-risk work).
- Backlog rebuild: fragment file layout + ID scheme + the archive/condense step (G10–G12).
- Whether harvesting is hook-driven (ECC `observe.sh` → `observations.jsonl` → instincts) or
  manual at item close.
- How much of `.claude/`-native memory we reuse vs. a wiki-resident knowledge format.
