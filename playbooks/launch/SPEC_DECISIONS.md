# Spec Decisions — /launch

## LD-001: PREFLIGHT is mandatory, not optional

**Date:** 2026-04-06
**Decision:** SHIP mode runs PREFLIGHT automatically before deploying. PREFLIGHT cannot be skipped.
**Why:** Deploying to production without verifying readiness is the single most common cause of bad deploys. The checklist takes seconds. A broken deploy costs hours of recovery, user confusion, and business owner trust. Making PREFLIGHT mandatory removes the temptation to "just push it real quick." The standalone PREFLIGHT mode exists so the business owner can check readiness without committing to deploy — but when SHIP runs, preflight happens whether you ask for it or not.

## LD-002: Production requires explicit confirmation

**Date:** 2026-04-06
**Decision:** Production deployments require the business owner to type "ship it" before /launch pushes code.
**Why:** The business owner is the authority on when their app goes live. An AI playbook pushing to production automatically — even after passing all checks — removes human judgment from a high-stakes decision. The confirmation is deliberately a phrase, not a button click, because it forces conscious intent. Staging can auto-deploy (lower stakes, easy to redo). Production is a one-way door in the eyes of users.

## LD-003: Three modes, not four

**Date:** 2026-04-06
**Decision:** PREFLIGHT, SHIP, and NOTES. No separate DEPLOY mode for pushing an already-packaged release.
**Why:** The reference shipper playbook had four modes (NOTES, PACK, SHIP, DEPLOY) because it was designed for a professional workflow where packaging and deploying were separate concerns. /launch serves business owners who want one action: "make it live." Splitting SHIP from DEPLOY creates a question ("did I ship or deploy?") that the audience shouldn't have to answer. SHIP does everything: preflight, notes, version, push, verify. NOTES exists for drafting. PREFLIGHT exists for dry runs. Three modes, three clear purposes.

## LD-004: No zip packaging

**Date:** 2026-04-06
**Decision:** /launch does not create zip packages. It pushes code via git to trigger platform deploys.
**Why:** The reference shipper playbook created zip deliverables because clients received files. The Playbooks audience deploys via git-push to hosting platforms (Render, Vercel). There's no zip to hand off — the hosting platform receives the code directly. Release notes and deploy logs are stored in `shipped/{version}/` for the record, but the deliverable is the live URL, not a file.

## LD-005: Destructive action prevention as hard rules

**Date:** 2026-04-06
**Decision:** Six non-negotiable rules: no force-push, no unverified targets, no blind pushes, no skipping confirmation, no deleting branches/tags, no amending published commits.
**Why:** AI agents have caused real damage through force-pushes and blind deploys in production environments. These aren't guidelines — they're hard stops. The cost of a 30-second confirmation is zero. The cost of deploying the wrong code to the wrong place is hours of recovery. Every rule on this list exists because the failure mode actually happened to someone. Prevention, not recovery.

## LD-006: Rollback command in every deploy log

**Date:** 2026-04-06
**Decision:** Every deploy log entry includes the exact command or instruction to revert to the previous version.
**Why:** When something breaks in production, the business owner needs to undo it fast. They shouldn't have to figure out how to rollback under pressure. The instruction is written at deploy time when the context is fresh — not discovered under stress when the app is down. This is insurance that costs nothing to create and everything to not have.

## LD-007: Verify the URL, not just the push

**Date:** 2026-04-06
**Decision:** After pushing, /launch checks that the live URL returns HTTP 200 (and optionally the correct version). A successful push with a broken URL is reported as a failure.
**Why:** "I pushed the code" is not the same as "the app is live." Build failures, environment misconfigurations, and platform issues can all cause a successful push to result in a broken app. The business owner cares about one thing: can their users reach it? /launch checks that.

---

## Template

```markdown
## LD-XXX: [Title]

**Date:** YYYY-MM-DD
**Decision:** What you decided.
**Why:** Why it made sense at the time.
```
