---
title: Open questions (unresolved forks)
type: question
status: draft
---

Live forks the wiki hasn't settled — for you to weigh in on:

- **Reuse measurement** ([retrieval](retrieval.md)) — the success metric has no instrumentation; today we
  only see retrieval in transcripts. A read-counter hook? Periodic prune review? Decide after more live runs.
- **Capture fatigue** ([capture](capture.md)) — if per-stop nudging trains reflex dismissals, soften how?
  (Cooldown, edit-count threshold, or batch-at-sprint-close.)
- **Confidence** — is it worth computing for [Vibin knowledge](the-atom.md) in practice, or does it become a
  number nobody updates?
- **Seed backport** — when to fold the visuals-proven machinery back into the Vibin seed as the real v2
  ([from wiki to machinery](from-wiki-to-machinery.md))?

**Resolved:** knowledge lives in **wiki atoms only, never native memory** — see
[what belongs](what-belongs.md). Superseded atoms are **deleted and repointed** — see
[the knowledge graph](the-graph.md). **Capture mechanism**: a deterministic hook that prompts the agent to
distil at close — implemented and proven (first run captured into an existing atom) — see
[capture](capture.md). **Strangle vs rebuild**: strangle — v2 was stood up *inside* a live v1 project
(visuals) and the v1 machinery removed piece by piece.

**Why:** naming the forks keeps them visible instead of silently decided. Each becomes a [decision](the-atom.md)
once settled.
