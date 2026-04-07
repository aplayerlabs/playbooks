# Playbook Blueprint

How to build a playbook that plugs into Playbooks. Use this if you want to create a custom playbook for your own domain.

## Terminology

- **Playbook** — the whole system. One playbook, threading all skills together through a shared playfield.
- **Skills** — the 12 slash commands (`/discover`, `/plan`, `/build`, etc.). Each skill does one job in the chain.
- **Plays** — the steps and moves within each skill (modes, protocols, decision trees).
- **Playfield** — the project folder structure that every skill operates through. Skills read it, run their plays, write back to it.

## What is a skill?

A skill is a specialist. One job, one mindset, clear inputs and outputs. It's implemented as a Claude Code slash command — a set of markdown files that define how Claude operates in a specific role.

Skills are not code. They're operating contracts. They define what the AI should do, how it should think, what it can and can't touch, and when it should refuse.

## How playbooks are designed

Every skill in Playbooks was designed by running the domain through six lenses from Gary Klein's Recognition-Primed Decision Making (RPM) model. When you build your own skill, use them to extract the expertise that makes it smart — not just the steps, but the thinking behind the steps.

### The six lenses

1. **Pattern Recognition** — What patterns does the expert see that others miss? What signals tell them "this is going well" vs "this is about to go wrong"?
2. **Cognitive Tasks** — What mental steps do they go through? Not the physical steps, but the thinking: assess, compare, prioritise, cross-reference, infer.
3. **Heuristics** — What rules of thumb guide their decisions? "If X, always Y." "When in doubt, Z." These are the shortcuts that make experts fast.
4. **Situational Sensitivity** — What cues tell them the situation has changed? When do they shift approach? What triggers a different mode of thinking?
5. **Problem Models** — How do they solve recurring problems? What's their mental playbook for common scenarios?
6. **Instructional Design** — If teaching a complete novice, what would they teach first? What's essential vs nice-to-have?

### How the lenses map to CLAUDE.md sections

| Lens | Maps to |
|------|---------|
| Pattern Recognition | **Mindset** — what the playbook values, what it watches for |
| Heuristics | **Heuristics section** — rules of thumb that make decisions fast |
| Situational Sensitivity | **Domain sections** with conditional behavior — "when X, shift to Y" |
| Problem Models | **Playbooks** — reusable solutions to recurring scenarios |
| Cognitive Tasks | **Operating modes** — distinct thinking patterns for different situations |
| Instructional Design | **STATUS.md updates** — how the playbook communicates with the business owner |

The lenses are an extraction tool. Sit with the domain expert (or be the domain expert) and work through each lens. The answers become the skill's CLAUDE.md. Skip the lenses and you get a playbook that follows steps. Use them and you get a playbook that thinks.

## Required files

Every skill lives in `playbooks/{playbook-name}/` and contains:

```
playbooks/my-playbook/
  SKILL.md              ← Registers the skill as a slash command
  CLAUDE.md             ← The operating contract
  USAGE.md              ← How to invoke it
  SPEC_CHANGELOG.md     ← Version history
  SPEC_DECISIONS.md     ← Why the rules exist
  playbooks/            ← Optional. Reusable patterns.
```

## SKILL.md

Registers the skill as a Claude Code slash command. See [SKILL-FORMAT.md](SKILL-FORMAT.md) for the full frontmatter spec.

```yaml
---
name: my-playbook
version: 1.0.0
description: |
  One sentence: what this playbook does.
position-in-pipeline: 0
voice-triggers:
  - "my playbook"
allowed-tools:
  - Bash
  - Read
  - Write
  - Edit
---
```

Below the frontmatter, include the skill content — typically instructions to read and follow the skill's CLAUDE.md.

## CLAUDE.md — the operating contract

This is the skill spec. Everything else is supporting material. Required sections:

### 1. Role
One sentence. What this skill does.

```markdown
## Role
Build the application from the product requirements document.
```

### 2. Mindset
3-5 bullets. How this skill thinks. What it values. What it refuses to compromise on.

```markdown
## Mindset
- Correctness over speed. If it's not right, it's not done.
- Every change maps to a requirement. No cowboy coding.
- The business owner reads STATUS.md. Keep it honest and clear.
```

### 3. Operating modes
2-4 distinct modes. Each mode has a different permission level and behavior.

```markdown
## Operating Modes

### BOOTSTRAP
First session only. Read PRD, scaffold the playfield, create initial tasks.

### BUILD (default)
Execute tasks autonomously. Write code, update docs, track progress.

### FIX
Targeted bug fixes from /test findings.
```

Mode design principles:
- 2-4 modes maximum. More than 4 means the playbook is doing too many jobs.
- One default mode. If the user doesn't specify, this is what runs.
- Clear triggers. The user should know which mode to pick without reading docs.
- Distinct behaviors. If two modes feel similar, merge them.

### 4. Session start protocol
What the skill reads when it starts a session.

```markdown
## Session Start
1. Check for SESH.md. If missing, create it with all section headers.
2. Look around the project for artifacts from upstream playbooks — PRD, deploy.json,
   package.json, design files, running app. Read what's there.
3. Backfill SESH.md from whatever exists. Extract problem statements, scope,
   design notes, infra config — anything upstream playbooks would have written.
4. Read STATUS.md — understand what the business owner has been told.
5. Read any playbook-specific files (PRD, tasks, bugs, etc.)
6. Flag gaps honestly: what's missing from upstream, what you'd recommend.
7. Orient the user: "Here's where we are. Here's what I'll do next."
```

The pattern is: **read the room, backfill from whatever exists, flag gaps, keep moving.** Never block because upstream data is missing.

### 4a. Re-entry protocol
What happens when this playbook has already run on this project (its SESH.md section is populated):

```markdown
## Re-entry
1. Acknowledge existing work: "You already have [X] from a previous session."
2. Offer options: refine (iterate), restart (clear and redo), or skip (next playbook).
3. Default to refine. Never silently overwrite prior work.
```

### 5. Domain-specific sections
The meat of the playbook. Rules, responsibilities, heuristics. Whatever the playbook needs to do its job. This varies entirely by playbook.

### 6. What this skill does NOT do
Explicit boundaries. Prevents scope creep.

```markdown
## What /build Does NOT Do
- Approve releases (that's the business owner's decision)
- Configure hosting infrastructure (that's /setup)
- Run tests with a breaker mindset (that's /test)
- Make product decisions (that's the business owner via /define)
```

### 7. Refusal conditions
When the playbook must stop and say no.

```markdown
## Refusal Conditions
- Requirements are missing or ambiguous — suggest /define
- Work doesn't map to any requirement — refuse, explain why
- Asked to do something outside this playbook's domain — name the right playbook
```

### 8. Self-modification rules
How the playbook's own CLAUDE.md can be changed.

```markdown
## Self-Modification Rules
This playbook MAY update its own CLAUDE.md if:
- Change is committed as an isolated commit
- Commit message starts with [CLAUDE.md]
- No other files are included
- Change is explained first

This playbook MUST NOT modify:
- Refusal Conditions (section 7)
- Self-Modification Rules (section 8)
```

### 9. Operating principle
One line. The playbook's philosophy.

```markdown
## Operating Principle
Ship working software, not perfect software.
```

## USAGE.md

How to invoke the skill. Include:

- Where the playbook lives (don't copy it into projects)
- How to start a session (one example per mode)
- What goes where (which files live in the skill folder vs the project)
- How this skill works with other playbooks in the pipeline

## SPEC_CHANGELOG.md

Track changes to the skill spec itself (not project changes).

```markdown
# Spec Changelog

## v1.0.0 — 2026-04-06
- Initial release
- Modes: BOOTSTRAP, BUILD, FIX
- 12 sections in CLAUDE.md
```

## SPEC_DECISIONS.md

Why the rules exist. Decisions that shaped the skill's design.

```markdown
# Spec Decisions

## BD-001: Three modes, not one
**Date:** 2026-04-06
**Decision:** Separate BOOTSTRAP, BUILD, and FIX into distinct modes.
**Why:** Different permission levels. BOOTSTRAP scaffolds (destructive if re-run). BUILD is autonomous. FIX is targeted. Mixing them risks accidental scaffolding during a bug fix.
```

Use a unique prefix per skill (BD for build, TD for test, DD for design, etc.).

## Playbooks (optional)

Reusable patterns specific to this skill. Stored in the skill folder, not in projects.

```
playbooks/build/playbooks/
  frontend-patterns.md
  database-patterns.md
  auth-patterns.md
```

Playbooks are IP that travels with the playbook across all projects.

## Playfield integration

Every skill that participates in the pipeline operates on the **playfield** — the shared project folder structure:

1. **Read SESH.md on start** — understand where the project is
2. **Write to its own section** — don't overwrite other skills' sections
3. **Write the progress block** — Status, Completed, Files Changed, Next Up, Blockers
4. **Update STATUS.md** — plain English for the business owner

If the skill creates SESH.md (because it's the first skill used on a project), it should initialize all section headers so future skills can find their sections. The full playfield (tasks/, bugs/, docs/, etc.) is scaffolded by `/build` BOOTSTRAP.

## Direct entry

Every skill must handle being entered directly (not through the pipeline):

1. No SESH.md? Create it.
2. Missing data from a prior stage? Tell the user what's missing and suggest which skill to run. Don't block — adapt and work with what's available.
3. Has everything it needs? Proceed normally.

## Checklist

Before shipping a skill:

**Files:**
- [ ] SKILL.md has valid frontmatter
- [ ] CLAUDE.md has all required sections (Role, Mindset, Modes, Session Start, Re-entry, domain sections, Boundaries, Refusal, Self-modification, Operating Principle)
- [ ] USAGE.md exists with examples for each mode
- [ ] SPEC_CHANGELOG.md has at least v1.0.0
- [ ] SPEC_DECISIONS.md has at least one decision

**Pipeline awareness:**
- [ ] Playbook reads SESH.md on start
- [ ] Playbook creates SESH.md with all section headers if missing
- [ ] Playbook backfills SESH.md from existing project artifacts (PRD, deploy.json, code, etc.)
- [ ] Playbook writes to its own SESH.md section (doesn't overwrite others)
- [ ] Playbook updates STATUS.md in plain English
- [ ] Playbook knows what playbook comes before and after it in the pipeline
- [ ] Playbook flags missing upstream data honestly but doesn't block

**Direct entry:**
- [ ] Playbook handles no SESH.md (creates it, backfills from project)
- [ ] Playbook handles missing prior stage data (flags gaps, adapts, proceeds)
- [ ] Playbook handles re-entry (section already populated — offers refine/restart/skip)

**Guard rails:**
- [ ] Playbook has explicit refusal conditions
- [ ] Playbook has explicit boundaries (what it does NOT do)
- [ ] Playbook triggers auto-wrap when context is running low
