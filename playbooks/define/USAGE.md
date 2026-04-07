# Usage — /define

## Where it lives

```
~/.claude/skills/playbooks/playbooks/define/
```

Do not copy skill files into projects. /define reads from the skill folder and writes to your project directory.

## How to start

### First time (DISCOVER mode)

```
/define
```

If no PRD exists, /define starts a structured conversation to figure out what your app needs to do. Six phases, one question at a time:

1. **Solution** — what are we building and how does it work?
2. **Scope** — what's in the first version, what waits?
3. **Data** — where does the data come from, what's calculated?
4. **Experience** — what does the user actually do, step by step?
5. **Deployment** — where does this live?
6. **Constraints** — any hard limits or risks?

Each question comes with options and a recommendation. You pick, riff, or go your own way.

### Have reference files?

Drop spreadsheets, PDFs, or screenshots into a `reference/` folder in your project before starting. /define reads them directly and extracts data specs, formulas, and structure — you don't need to explain what's in them.

### Write the PRD (DRAFT mode)

After the six phases are done:

```
/define DRAFT
```

Or just say "write the PRD" during the conversation. /define writes `prd/prd.md` with everything documented: problem, solution, scope tables, data spec, user flow, deployment targets, stages, constraints, and success criteria.

### Review an existing PRD (CHALLENGE mode)

```
/define CHALLENGE
```

Stress-tests your PRD for gaps, contradictions, and missing edge cases. Produces a gap report.

### Update the PRD (REFINE mode)

```
/define REFINE
```

Surgical edits to specific sections. New scope items, revised data specs, updated constraints — without rewriting the whole document.

### Re-entry

```
/define
```

If a PRD already exists, /define offers:
- **Refine** — update specific sections
- **Restart** — clear and re-discover from scratch
- **Skip** — move to /design

## What goes where

| File | Location | Purpose |
|------|----------|---------|
| `prd/prd.md` | Project root | The product requirements document |
| `reference/` | Project root | Your source files (spreadsheets, PDFs, images) — read-only |
| `SESH.md` | Project root | Requirements section updated by /define |
| `STATUS.md` | Project root | Plain English progress |

## In the pipeline

```
/discover → /plan → /setup → [ /define ] → /design → /build → /test → /launch
```

/define works best after /discover (validated problem), /plan (stress-tested direction), and /setup (infrastructure configured). But it can be entered directly — if prior stages haven't run, /define gathers the essentials itself and flags what's missing.

## The scope gate

/define will not write a PRD until you confirm the scope tables — which features are in the first version and which are deferred. This is the single most important decision in the whole pipeline. Take your time with it.
