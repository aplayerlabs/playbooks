# Spec Decisions

## WD-001: Two modes (WRAP + AUTO), not one
**Date:** 2026-04-06
**Decision:** Separate manual WRAP from system-triggered AUTO.
**Why:** AUTO must skip the user prompt and save immediately — context is running out, there's no time for conversation. WRAP can take a beat, confirm with the user, and be more thorough. Same process, different entry behavior.

## WD-002: Continuation prompt points to a playbook, not just "resume"
**Date:** 2026-04-06
**Decision:** The continuation prompt explicitly names the target skill and mode, rather than saying "resume where you left off."
**Why:** The next session is a blank slate. It doesn't know what "resume" means. By naming the playbook, the mode, and the exact stopping point, the continuation prompt gives the new session everything it needs to start working in under 30 seconds. It also handles backward movement — /test wrapping to /build FIX — which a generic "resume" prompt can't express.

## WD-003: Plan-mode continuation over copy-paste prompts
**Date:** 2026-04-06
**Decision:** /wrap presents a Claude Code plan instead of outputting a text block for the user to copy-paste into a new session.
**Why:** Copy-paste continuation prompts require the business owner to manually start a new session and paste text — friction that breaks flow and confuses non-technical users. A Claude Code plan leverages `showClearContextOnPlanAccept`: the user accepts the plan, context clears automatically, and execution resumes fresh with SESH.md as the source of truth. Same state preservation, zero manual steps. The plan mechanism handles within-session context refresh; SESH.md handles across-session resume (user closes Claude Code entirely, comes back later, types /app).

## WD-004: /wrap never sets Status: DONE
**Date:** 2026-04-06
**Decision:** /wrap always writes `Status: CONTINUING`, never `Status: DONE`.
**Why:** DONE means the skill's work is complete and the pipeline should advance. That's the active skill's call, not /wrap's. /wrap is a pause button, not a finish button. If the skill's work was truly done, it would have set DONE itself before /wrap was ever needed.
