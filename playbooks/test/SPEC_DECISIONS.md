# Spec Decisions — /test

## TT-001: Four modes, not one general "test" mode

**Date:** 2026-04-06
**Decision:** Separate HUNT, VERIFY, REGRESS, and EDGE into distinct modes rather than a single testing flow.
**Why:** Different testing needs require different mindsets and strategies. Exploratory hunting is open-ended and systematic. Targeted verification is focused and binary. Regression testing requires reading bug history and attempting reproduction. Edge case testing requires paranoid creativity and playbook consultation. A single "test" mode would either be unfocused (try everything every time) or require complex sub-routing. Explicit modes make intent clear and behavior predictable.

## TT-002: /test never fixes bugs

**Date:** 2026-04-06
**Decision:** /test only finds and reports. Never fixes bugs, never writes or modifies production code.
**Why:** The breaker mindset and the fixer mindset are in tension. A playbook that both finds and fixes might soften reports to avoid creating work for itself, or might fix obvious issues instead of documenting them — leaving no record. Clean separation also prevents /test from introducing new bugs while "fixing" old ones. The loop-back protocol (/test finds, /wrap to /build FIX, /wrap back to /test VERIFY) keeps both playbooks doing what they do best.

## TT-003: Ship readiness signal, not ship approval

**Date:** 2026-04-06
**Decision:** /test reports ship readiness (GREEN/YELLOW/RED) but never approves or recommends shipping.
**Why:** The business owner decides when to ship. /test provides evidence: what was tested, what broke, how severe. The decision to accept YELLOW risk or delay for GREEN belongs to the human. If /test could say "you should ship," it would be making a business decision it's not qualified to make. If it could say "you shouldn't ship," it would be overriding the business owner's authority. Report the facts. Let the human decide.

## TT-004: Executive summary is the primary output

**Date:** 2026-04-06
**Decision:** Every test session produces a plain-English executive summary in STATUS.md with ship readiness, bug counts, and top blocker.
**Why:** The business owner doesn't read `bugs/open.md`. They read STATUS.md. If the executive summary isn't there, the testing results are invisible to the decision-maker. The detailed bug reports exist for /build to fix — the summary exists for the business owner to decide. Two audiences, two outputs, both required.

## TT-005: Coverage tracking with confidence levels

**Date:** 2026-04-06
**Decision:** Track not just what was tested, but confidence level (HIGH/MEDIUM/LOW/UNTESTED) for each feature.
**Why:** "Tested" is not binary in exploratory testing. There's a difference between "I tried hard to break the login flow and couldn't" (HIGH) and "The login page loaded" (LOW). Without confidence levels, a coverage checklist creates false confidence — everything has a checkmark but half of it was barely glanced at. Confidence levels force /test to be honest about depth.

## TT-006: Plays live in the skill, not in projects

**Date:** 2026-04-06
**Decision:** Testing plays (edge cases, security, performance, accessibility) ship with the skill, not in individual project folders.
**Why:** Testing knowledge accumulates across projects. The edge cases that matter for one client's login form matter for every login form. Plays are IP that travel with the skill — reusable patterns that make every test session more thorough than the last. If they lived in projects, they'd fragment and diverge.

## TT-007: No automated test execution

**Date:** 2026-04-06
**Decision:** /test does exploratory testing through code review and systematic analysis. It does not write or execute unit tests, integration tests, or end-to-end test suites.
**Why:** Automated test writing is /build's responsibility — tests are part of the codebase. /test is the second pair of eyes: the adversarial reviewer who finds what automated tests miss. Edge cases, UX issues, security holes, accessibility gaps, and "nobody would ever do that" scenarios are /test's domain. Automated tests verify known expectations; /test discovers unknown failures.

---

## Template

```markdown
## TT-XXX: [Title]

**Date:** YYYY-MM-DD
**Decision:** What you decided.
**Why:** Why it made sense at the time.
```
