# Spec Decisions — /discover

## DC-001: Two modes (PROBLEM and IDEA), not one

**Date:** 2026-04-06
**Decision:** Separate PROBLEM and IDEA into distinct modes rather than a single discovery flow.
**Why:** Business owners arrive in two very different mental states. Someone with a pain point needs validation — "yes, this is real." Someone with an idea needs redirection — "what problem does this solve?" The questioning strategy differs. PROBLEM mode validates forward; IDEA mode works backward. Merging them creates a muddled experience where the brain doesn't know which direction to push.

## DC-002: Only two Question Engine phases, not eight

**Date:** 2026-04-06
**Decision:** Adapt only phases 1 (Reality) and 2 (Outcome) from the Definer's Question Engine. Phases 3-8 (Solution, Scope, Data, Experience, Deployment, Constraints) belong to /define.
**Why:** /discover's job is problem validation, not product definition. The Definer brain handles the full 8-phase engine because it writes the PRD. /discover only needs to establish ground truth (what's the problem, who has it, what do they do today) and success criteria (what does solved look like). Going further would overlap with /plan (stress-testing direction) and /define (writing requirements). Clean separation: /discover owns the problem, /define owns the solution.

## DC-003: Business owner language, not consultant language

**Date:** 2026-04-06
**Decision:** All questions use plain English with concrete options. No frameworks, no jargon, no acronyms.
**Why:** The audience is technical-adjacent business owners, not product managers or engineers. The Definer's Question Engine was designed for a professional consulting context. /discover is designed for someone who has never been through a product discovery process. "What's painful about the current situation?" beats "Let's map the current-state workflow and identify friction points." Same outcome, different language. The options pattern (pick 1, 2, or 3) reduces cognitive load — the business owner reacts rather than generates.
