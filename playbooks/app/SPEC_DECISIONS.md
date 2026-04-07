# Spec Decisions

## RD-001: Read-only, no exceptions
**Date:** 2026-04-06
**Decision:** The router playbook never writes to SESH.md, STATUS.md, or any project file.
**Why:** If the router could modify state, it becomes a worker with routing bolted on. That creates ambiguity about what changed and who changed it. Every pipeline playbook trusts that SESH.md reflects the work of the skill that wrote it. A router that writes breaks that trust chain.

## RD-002: One mode, not multiple
**Date:** 2026-04-06
**Decision:** Single ROUTE mode instead of separate modes for new projects, existing projects, and blocked projects.
**Why:** The router's behavior branches on project state, not on user intent. The user always wants the same thing: "where am I, what's next." Separate modes would force the user to know their state before asking — which defeats the purpose of a router.

## RD-003: Recommend, don't auto-launch
**Date:** 2026-04-06
**Decision:** The router suggests the next skill but does not automatically invoke it.
**Why:** The business owner should always feel in control. Auto-launching a playbook removes their ability to ask questions, change direction, or check something first. The recommendation gives them the answer; the decision to act stays with them.
