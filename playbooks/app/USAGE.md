# /app — Usage

## Where it lives

```
playbooks/playbooks/
  SKILL.md
  CLAUDE.md
  USAGE.md
  SPEC_CHANGELOG.md
  SPEC_DECISIONS.md
```

Do not copy these files into your project. The skill system reads them from the playbooks directory.

## How to invoke

```
/app
```

Or say: "start", "what should I do", "a player playbooks"

## What happens

The skill reads your project directory and tells you where you are:

**New project (no SESH.md):**
```
Fresh start. Let's find the real problem first. Type /discover to begin.

Pipeline: /discover -> /plan -> /setup -> /define -> /design -> /build -> /test -> /launch
```

**Existing project (SESH.md found):**
```
You're at the Design stage. Problem is validated, plan is set, infra is configured,
and requirements are defined. I'd suggest running /design next.
```

**Mid-session resume (Status: CONTINUING):**
```
You were mid-session with /build. Pick up where you left off?
Last working on: TASK-012 — Date range picker component.
```

**Blocked project (Status: BLOCKED):**
```
This project is blocked. Reason: Need Stripe API key to implement payment flow.
Once that's resolved, resume with /build.
```

## How it works with the pipeline

This skill sits outside the pipeline. It reads the project state and points you to the right skill. It never modifies anything.

Think of it as the reception desk. You walk in, it looks up your file, and tells you which specialist to see next.

## Modes

One mode: ROUTE. It reads and recommends. That's it.
