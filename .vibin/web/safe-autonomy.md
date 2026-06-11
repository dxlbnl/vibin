---
title: Safe autonomy (how far an agent runs alone)
type: principle
status: draft
---

Agents can run unattended — within guardrails, with the human holding a clear **control dial**.

- **Autonomy dial** — run-until-blocked for routine work; **stop and ask** at risk, ambiguity, or an
  architectural fork ([right-sizing](right-sizing.md) heavy path).
- **Security is a standing concern** — secrets never enter the wiki or logs; least-privilege tool permissions;
  don't execute untrusted code; supply-chain caution.
- **The human steers mid-flight** — a conversation ([the conversational track](the-conversational-track.md)) or
  a checkpoint can redirect without unwinding everything.

**Why:** an autonomous, knowledge-compounding system that is careless with secrets or runs past its mandate is a
liability. Safe autonomy is table-stakes for "grand"; old Vibin never addressed it.
