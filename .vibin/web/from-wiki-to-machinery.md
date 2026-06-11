---
title: From wiki to machinery (deriving agents, skills, hooks)
type: mechanism
status: draft
---

The wiki is the spec; the implementation is Claude Code machinery. Every mechanism in the wiki lands as exactly
**one** of four surfaces, chosen by *what kind of thing it is*:

- **A gate → a hook.** Anything that must be *enforced*, not optional. From [gates](gates.md): read-before-work,
  [capture](capture.md)-before-close (the keystone), green-means-green ([verification](verification.md)).
- **A capability or loop-phase → a skill.** A reusable workflow run *inline*: the light and heavy tracks
  ([right-sizing](right-sizing.md)), [the conversational track](the-conversational-track.md), capture/distil,
  [retrieval](retrieval.md), [verification](verification.md), the [retro](the-retro.md), intake (GitHub issues → [the backlog](the-backlog.md)), and
  [bootstrapping](bootstrapping-the-web.md). Skills are the primary surface ([onboarding](onboarding-agents-and-skills.md)).
- **A recurring, distinct role → an agent.** Only roles worth a **cold spawn**
  ([onboarding](onboarding-agents-and-skills.md)) — kept few; most work is the main agent + skills, not a cold
  relay ([pain P2](../research/vibin-painpoints.md)).
- **A boundary → config.** [safe autonomy](safe-autonomy.md) and [cost & models](cost-and-models.md) →
  permission profile + model routing in settings.

**Build order:** the **keystone first** ([capture](capture.md) + atom format + [retrieval](retrieval.md)), proven
on one real task; then the light track + a verify gate; then the rest as needed — strangling old Vibin in place
([open questions](open-questions.md)). Seed an existing project's wiki with [bootstrapping](bootstrapping-the-web.md).

**Ongoing:** new skills/rules are **authored** (a curated library) or **evolved from use** via the
[retro](the-retro.md) — concrete steps, human-reviewed ([self-improvement](self-improvement.md)), never automatic.

**Why:** the wiki describes *what* Vibin is; this atom describes *how it becomes hooks, skills, agents, and
config*. Without it, the design doesn't tell you what to build.
