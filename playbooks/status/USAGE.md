# /status — Usage

## Where it lives

```
playbooks/status/
  SKILL.md
  CLAUDE.md
  USAGE.md
  SPEC_CHANGELOG.md
  SPEC_DECISIONS.md
```

Do not copy these files into your project. The skill system reads them from the playbooks directory.

## How to invoke

```
/status
```

Or say: "where am I", "what's next", "status"

## What happens

The skill reads your project's SESH.md and STATUS.md and gives you a plain English report.

**No project found:**
```
No project state found here. Type /app or /discover to start a new project.
```

**Project in progress:**
```
## Your Project: Dashboard

Pipeline:
/discover [DONE] -> /plan [DONE] -> /setup [DONE] -> /define [CURRENT] -> /design -> /build -> /test -> /launch

Where you are: Requirements — defining exactly what to build.

What's done:
- Problem: Retail managers can't see stock levels across locations in real time.
- Direction: Single-page dashboard with Shopify integration. MVP first.
- Infrastructure: GitHub repo created, Render hosting configured.

What's next: Run /define to finish the product requirements document.

Blockers: None
```

**Paused mid-session:**
```
## Your Project: Dashboard

Pipeline:
/discover [DONE] -> /plan [DONE] -> /setup [DONE] -> /define [DONE] -> /design [DONE] -> /build [CURRENT] -> /test -> /launch

Where you are: Build — in progress, paused mid-session.

Last session: You were working with /build on TASK-012 (date range picker).
Left off at: Implementing validation in src/components/DateFilter.tsx.

What's next: Resume /build to finish TASK-012.

Blockers: None
```

## Modes

One mode: STATUS. It reads and reports. That's it.

## How it works with the pipeline

/status is a window into your project. It reads everything, writes nothing. Use it any time you want to know where things stand before deciding what to do next.

Works well paired with /app — /status tells you where you are, /app tells you where to go.
