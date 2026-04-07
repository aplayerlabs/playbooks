# Playbooks — Public Release Plan

## What this is

A skill pack for Claude Code that takes a business owner from "I have a problem" or "I have an idea" to a shipped, working application — without needing to know how to code.

## Who it's for

The technical-adjacent business owner. Comfortable enough with a computer to follow instructions in a terminal, but has never written code. They've identified a bottleneck in their business and want software to give them leverage through it — or they have an idea and need to figure out if it's worth building.

## Guiding principles

1. **Problem-first.** Whether they arrive with an idea or a problem, they leave /discover with a validated problem.
2. **Linear skill chain.** One playbook at a time, in order. No complex routing.
3. **Train tracks with guard rails.** The business owner can't go wrong if they follow the sequence.
4. **/wrap closes every loop.** Need to go back? Wrap, and the continuation prompt points to the right playbook. Re-entry is always clean.
5. **Plain English always.** Every playbook produces output the business owner can read. Technical artifacts exist underneath for skill-to-skill coordination, but the human reads STATUS.md.
6. **Opinionated defaults.** We choose the stack, the hosting, the structure. The business owner makes product decisions, not technical ones.
7. **Every playbook stands alone.** The skill chain is the recommended path, but any playbook can be entered directly. If SESH.md doesn't exist, the playbook creates it. If it does exist, the playbook reads it and continues.
8. **Everything written new.** Existing Playbooks playbooks are design input. Nothing is copied.

---

## The skill chain

```
/app (front door — reads state, routes to the right playbook)
    ↓
/discover → /plan → /setup → /define → /design → /build → /test → /launch
```

**Utilities:** /wrap, /status, /upgrade

**Total slash commands:** 11

---

## Shipping schedule

One skill chain. Built and released in phases based on dependency order:

| Phase | Playbooks | Why this order |
|-------|--------|----------------|
| **Phase A** | Repo architecture, meta-infrastructure, /app, /discover, /plan, /wrap, /status, /upgrade | Foundation + front door + the thinking playbooks. Useful immediately. Validates audience via YouTube. |
| **Phase B** | /setup, /define, /design | Infrastructure + requirements + visual design. These three prepare everything needed before code is written. |
| **Phase C** | /build, /test, /launch | The building, breaking, and shipping playbooks. Heaviest lift. Completes the skill chain. |

Each phase ships when ready. /upgrade delivers new playbooks to existing users.

---

# PHASE 1: REPO ARCHITECTURE & PACKAGING

*Foundation. Everything depends on this.*

| # | Task | Detail | Deliverable |
|---|------|--------|-------------|
| 1.1 | **Repo structure** | New public repo. Clone into `~/.claude/skills/apb`. Top-level: `setup`, `README.md`, `ARCHITECTURE.md`, `ETHOS.md`, playbook folders, `youtube/`, `examples/`. | Repo skeleton |
| 1.2 | **Setup script** | One-command install. Registers all playbooks and utilities as slash commands. Detects host (Claude Code primary). Extensible as new playbooks ship. | `setup` executable |
| 1.3 | **Skill file format spec** | Every playbook gets a `SKILL.md` with YAML frontmatter: name, description, voice-triggers, allowed-tools, version, position-in-pipeline. Business-owner-friendly descriptions. | `SKILL-FORMAT.md` |
| 1.4 | **Composition spec** | How playbooks invoke each other. How /plan runs thinking tools internally. How /wrap generates continuation prompts naming the next playbook. How /app reads state and routes. | Section in `ARCHITECTURE.md` |

---

# PHASE 2: META-INFRASTRUCTURE

*The architecture that makes all playbooks interoperable. Must exist before individual playbooks.*

| # | Task | Detail | Deliverable |
|---|------|--------|-------------|
| 2.1 | **Playbook blueprint** | How to build a playbook. Required files (SKILL.md, CLAUDE.md, USAGE.md, SPEC_CHANGELOG.md, SPEC_DECISIONS.md). Required CLAUDE.md sections. Mode design principles. Written for public playbook authors — doubles as "Build Your Own Playbook" guide. Reference: existing `a-player-playbook-blueprint.md`. | `PLAYBOOK-BLUEPRINT.md` |
| 2.2 | **SESH.md spec** | Playbook-to-playbook handoff contract. Created by whichever playbook is entered first. Updated by every playbook. Structured markers: status (DONE/CONTINUING/BLOCKED/ERROR), blocker format, files changed, next-up. Regex patterns for parsing. Travels the entire skill chain from first touch. Reference: existing `progress-signaling.md`. | `PROGRESS-SIGNALING.md` + template in `playfield-template/` |
| 2.3 | **STATUS.md spec** | Plain-English file the business owner reads. Every playbook updates it alongside SESH.md. Contains: project name, current skill chain stage, what's been decided/built/tested, what's next, blockers in plain language. Later stages add: ship readiness (RED/YELLOW/GREEN), version, deployment status. | Template in `playfield-template/` + spec in `ARCHITECTURE.md` |
| 2.4 | **Handoff protocol** | How each playbook hands forward via SESH.md. How /wrap generates continuation prompts. Rules for backward re-entry (wrap, change the playbook name, paste). | Section in `ARCHITECTURE.md` |
| 2.5 | **Direct entry protocol** | What happens when someone enters a playbook directly without going through the skill chain. Every playbook checks: does SESH.md exist? If yes, read it and continue. If no, create it. Does the playbook have what it needs from prior stages? If no, tell the user what's missing and suggest which playbook to run first — but don't block them. | Section in `ARCHITECTURE.md` |
| 2.6 | **SESH.md accumulation spec** | What each playbook writes into SESH.md so nothing gets lost or overwritten as the document travels the skill chain. Each playbook owns its section. | Section in `ARCHITECTURE.md` |
| 2.7 | **Config system spec** | User-local config at `~/.apb/config.yaml`. Stores: hosting platform, GitHub username/org, branch strategy, project registry. Created by /setup. Read by /define, /design, /build, /launch. Playbooks that run before /setup don't need it. | Spec in `ARCHITECTURE.md` |

**What each playbook writes to SESH.md:**

| Playbook | Creates/Updates | Section it owns |
|-------|----------------|-----------------|
| /discover | Creates SESH.md + STATUS.md if they don't exist | `## Problem` — validated problem statement, who/what/why |
| /plan | Updates | `## Direction` — risks, guardrails, recommended approach, pre-decided responses |
| /setup | Updates | `## Infrastructure` — platform, repo, branch mapping, deploy config location |
| /define | Updates | `## Requirements` — PRD location, scope summary (in/out), stage breakdown |
| /design | Updates | `## Design` — design decisions, Figma file links, token set location |
| /build | Updates | `## Build` — architecture decisions, current task, files changed, progress |
| /test | Updates | `## Testing` — coverage, bugs found, severity summary, ship readiness |
| /launch | Updates | `## Deployment` — version, URL, deploy log location, rollback command |

Every playbook also writes the standard progress signaling block (Status, Completed This Session, Next Up, Blockers).

---

# PHASE 3: PIPELINE PLAYBOOKS

*The 8 playbooks, plus the front door and 3 utilities. Written new. Existing Playbooks playbooks are reference only.*

*Every playbook is chain-aware. It knows what comes before and after it. When entered directly without prior skill chain state, it reads the room — looks for PRDs, deploy configs, running apps, existing docs — backfills SESH.md from whatever exists, flags gaps honestly, and keeps moving. When re-entered on a project where it's already run, it offers to refine, restart, or skip. No playbook blocks because upstream data is missing.*

---

### 3.0 — `/app`

**Role:** Front door. Reads project state and routes to the right playbook.

| Aspect | Detail |
|--------|--------|
| **When** | Anytime. The one command the business owner needs to remember. |
| **Behavior** | No SESH.md found → "Looks like a new project. Let's start with /discover." Kicks off /discover. SESH.md exists → reads it, determines current skill chain stage, tells the user where they are in plain English, suggests next playbook. After /wrap → reads continuation state, offers to resume. |
| **Output** | Routes to the appropriate playbook. Updates nothing itself — it's a router, not a worker. |
| **Edge case** | Multiple projects in different stages → asks which project, or reads cwd to determine. |

---

### 3.1 — `/discover`

**Role:** Entry point for every project. Validates a problem or refines an idea into one.

| Aspect | Detail |
|--------|--------|
| **When** | Start of every new project |
| **Modes** | **PROBLEM** — User has a specific pain point. Validate: who has it, how often, what do they do today, why does it hurt? **IDEA** — User has a concept. Force problem-first: "who would use this and what problem does it solve for them?" Refine until there's a validated problem underneath. |
| **Input** | Conversation. Can also ingest assets (screenshots, spreadsheets, PDFs of existing workflows). |
| **Creates** | SESH.md + STATUS.md (if they don't exist). This is where the project's living documents begin. |
| **Output** | Validated problem statement in STATUS.md. SESH.md `## Problem` section populated. Clear "the problem we're solving is X for Y because Z." |
| **Guard rails** | Cannot exit without a validated problem. If the idea has no clear problem underneath, says so honestly. |
| **Direct entry** | Works standalone. No prior state needed — this IS the starting point. |
| **Reference** | Definer's Question Engine phases 1-2 (Reality + Outcome) |
| **YouTube** | Episode 1 |

---

### 3.2 — `/plan`

**Role:** Stress-test the direction before committing to building.

| Aspect | Detail |
|--------|--------|
| **When** | After /discover |
| **Modes** | **FULL** (default) — Runs four thinking tools in sequence: premortem (what kills this?), inversion (what guarantees failure?), bottleneck (where's the constraint?), optionality (which approach keeps doors open?). **FOCUSED** — Business owner picks which tools to run. |
| **Input** | Validated problem from SESH.md. Conversation to explore approaches. |
| **Output** | SESH.md `## Direction` section: top 3 risks with pre-decided responses, guardrails, recommended approach. STATUS.md updated in plain English. |
| **Guard rails** | Cannot exit without at least premortem completed. Business owner must acknowledge top risks. |
| **Direct entry** | Works standalone. If no SESH.md, creates one and asks about the problem first (lightweight /discover inline). If SESH.md exists but `## Problem` is empty, suggests running /discover first but doesn't block. |
| **Reference** | Existing premortem, inversion, bottleneck-mitigation-tree, optionality-mapping skills |
| **YouTube** | Episode 2 |

---

### 3.3 — `/setup`

**Role:** One-time infrastructure setup + per-project deployment config.

| Aspect | Detail |
|--------|--------|
| **When** | After /plan. First time ever runs INFRA mode. Per-project runs PROJECT mode. |
| **Modes** | **INFRA** — GitHub account, hosting platform (Render recommended, Vercel alternative), domain basics, `~/.apb/config.yaml` created. Cost guidance: what's free, what costs money. Teaches .env pattern. Step-by-step — the business owner will need to do things outside Claude Code (create accounts, copy API keys). Clear instructions for every external step. **PROJECT** — Creates `deploy.json` for this project, maps branches to environments, verifies hosting connection. |
| **Input** | Conversation about what they have and need. |
| **Output** | INFRA: `~/.apb/config.yaml` populated. PROJECT: `deploy.json` in project root. SESH.md `## Infrastructure` updated. |
| **Opinionated defaults** | Render (simple, generous free tier, git-push deploys). GitHub (universal). Main = production, staging = staging. |
| **External steps documentation** | Must include: creating a GitHub account, creating a Render/Vercel account, generating API keys, connecting repo to hosting. These happen outside Claude Code — playbook provides clear step-by-step with what to click and where. |
| **Direct entry** | Works standalone. If no SESH.md, creates one. Useful for someone who just wants hosting set up. |
| **Reference** | Shipper's deploy.json, gstack's setup script |
| **YouTube** | Episode 3 |

---

### 3.4 — `/define`

**Role:** Turn the battle-tested direction into a sharp PRD.

| Aspect | Detail |
|--------|--------|
| **When** | After /setup |
| **Modes** | **DISCOVER** (default) — Structured conversation: Solution, Scope, Data, Experience, Deployment, Constraints. **DRAFT** — Write PRD from accumulated context. **CHALLENGE** — Stress-test existing PRD. **REFINE** — Surgical edits. |
| **Input** | SESH.md (problem + direction + infra config). Conversation. Source assets (spreadsheets, PDFs, screenshots). |
| **Output** | `prd/prd.md` with: overview, problem, solution, user, core flow, scope (in/out tables), data spec, UI description, deployment targets, stages, constraints, success criteria. SESH.md `## Requirements` updated. STATUS.md updated. |
| **Guard rails** | Cannot write PRD without scope confirmation (business owner signs off on in/out tables). Cannot proceed without deployment section (reads config from /setup). |
| **Asset ingestion** | Reads .xlsx, .csv, .pdf, .png/.jpg. Never modifies source files. |
| **Direct entry** | Works standalone. If no prior state, asks the core questions itself (lightweight discover + plan inline). If SESH.md has problem + direction, picks up from there. |
| **Reference** | Definer playbook (Question Engine phases 3-8, asset ingestion playbook) |
| **YouTube** | Episode 4 |

---

### 3.5 — `/design`

**Role:** Visual design the business owner can see and react to before code is written.

| Aspect | Detail |
|--------|--------|
| **When** | After /define, before /build. Business owner needs visual stimulus to validate they're on the right track before committing to code. |
| **Modes** | **MOCKUP** (default) — Generate visual mockups from PRD. Could be Figma (if they have it), HTML preview pages, or annotated wireframes. The point is: something visual the business owner can look at and say "yes that's it" or "no, change this." **DESIGN** — Full Figma design with tokens and layers (for those who use Figma). **REFINE** — Iterate on visual direction based on feedback. |
| **Input** | PRD + SESH.md direction/requirements. |
| **Output** | Visual mockups the business owner can review. Design decisions documented. SESH.md `## Design` updated. STATUS.md: "Here's what your app will look like." |
| **External steps documentation** | If using Figma: creating account, installing Scripter plugin, pasting code. If using HTML previews: how to open the file in a browser. Clear instructions for each path. |
| **Why before /build** | Building from a PRD alone risks building the wrong thing visually. The business owner needs to SEE it before code starts. Cheaper to change a mockup than refactor code. |
| **Without Figma** | Generates HTML/CSS preview pages that open in a browser. Still visual, still reactable, no Figma needed. This is the default path for the everyman. |
| **Direct entry** | Works standalone. Can design from a description without prior skill chain state. If SESH.md has PRD context, uses it. |
| **Reference** | Designer playbook (DTCG pipeline, Scripter patterns). Also: the need for a non-Figma path is NEW and has no existing reference. |
| **YouTube** | Episode 5 |

---

### 3.6 — `/build`

**Role:** Build the application. The heaviest playbook — runs across many sessions.

| Aspect | Detail |
|--------|--------|
| **When** | After /design (business owner has approved the visual direction) |
| **Modes** | **BOOTSTRAP** — First session. Reads PRD + design decisions, makes architecture choices, scaffolds the full playfield (tasks/, bugs/, requirements/, roadmap/, docs/), creates Stage 0 tasks. Opinionated stack selection based on PRD. **BUILD** (default) — Execute tasks autonomously. Write code, update tasks, track progress. **FIX** — Targeted bug fixes from /test. |
| **Input** | PRD + design decisions + project doc structure. |
| **Output** | Working application. Updated tasks/, bugs/, docs/, CHANGELOG.md. STATUS.md with plain-English progress after every session. |
| **Stack decisions** | Made during BOOTSTRAP, documented in `docs/architecture.md`. Opinionated: Vite + React for client-side, Next.js for SSR, Tailwind for styling, Supabase if database needed. Business owner doesn't choose. |
| **Security** | Auth patterns enforced. .env for secrets. Input validation at boundaries. No secrets in code or git. |
| **Multi-session** | /wrap handles session boundaries. STATUS.md lets business owner track progress between sessions without reading code. |
| **Basic motion** | Bakes in sensible CSS transitions (hover, page transitions, loading) by default. |
| **Guard rails** | Every change maps to a task. Every task maps to a requirement. Won't build outside the PRD scope. |
| **Playfield** | BOOTSTRAP scaffolds the full playfield — the project folder structure every skill operates through. Earlier skills created SESH.md + STATUS.md; BOOTSTRAP adds everything else (tasks/, bugs/, requirements/, roadmap/, docs/). |
| **Direct entry** | Works standalone if pointed at a project with a PRD. If no playfield, runs BOOTSTRAP first. If no PRD, suggests /define but can work from a description (less structured). |
| **Reference** | Coder-v2 playbook (modes, doc structure, heuristics, situational sensitivity, architectural boundaries, bootstrap command) |
| **YouTube** | Episode 6 (Bootstrap) + Episode 7 (Multi-session building) |

---

### 3.7 — `/test`

**Role:** Break the application. Report what's wrong. Never fix.

| Aspect | Detail |
|--------|--------|
| **When** | After /build has a working feature or complete app |
| **Modes** | **HUNT** (default) — Exploratory, systematic, go deep. **VERIFY** — Targeted: "does X work?" **REGRESS** — Old bugs stay fixed. **EDGE** — Boundary conditions, abuse, security. |
| **Input** | Working app + requirements + existing bugs. |
| **Output** | Bugs in `bugs/open.md` with severity, repro steps, evidence. Executive summary in STATUS.md: "Ship readiness: RED/YELLOW/GREEN. X critical, Y medium, Z low. Top blocker: [plain English]." SESH.md `## Testing` updated. |
| **Ship readiness** | GREEN: no critical/high bugs, core flows tested, confidence HIGH. YELLOW: medium bugs, some paths untested. RED: critical/high bugs, blocks shipping. |
| **Guard rails** | Never fixes bugs (→ /build FIX via /wrap). Never approves shipping (human decides). Reports honestly. |
| **Loop** | /test finds bugs → /wrap → continuation to `/build FIX` → fixes → /wrap → continuation to `/test VERIFY` |
| **Playbooks** | Edge cases, security, performance, accessibility. |
| **Direct entry** | Works standalone on any running app. If no SESH.md, creates one. If no requirements, tests against reasonable expectations and notes the gap. |
| **Reference** | Tester playbook (4 modes, severity definitions, coverage tracking, playbooks) |
| **YouTube** | Episode 8 |

---

### 3.8 — `/launch`

**Role:** Package, version, deploy. Get it live.

| Aspect | Detail |
|--------|--------|
| **When** | After /test (GREEN or YELLOW with override) |
| **Modes** | **PREFLIGHT** — Verify: deploy.json exists, hosting connected, no critical bugs, STATUS.md GREEN/YELLOW. Report. **SHIP** (default) — Release notes, version bump, package, push to hosting, verify deployment, report. **NOTES** — Release notes only. |
| **Input** | Working tested app. deploy.json. |
| **Output** | Live app at their URL. Release notes in `shipped/{version}/`. CHANGELOG.md updated. STATUS.md: "v1.0.0 is LIVE at [url]." SESH.md `## Deployment` updated. |
| **Platform awareness** | Reads `~/.apb/config.yaml`. Render: push to branch, verify via API. Vercel: same. Generic: push, check URL. |
| **Guard rails** | PREFLIGHT before SHIP. Production requires explicit confirmation. Never force-push. Warns on critical bugs. Rollback documented. |
| **Direct entry** | Works standalone if deploy.json exists and there's code to ship. If missing config, suggests /setup. |
| **Reference** | Shipper playbook (release notes, packaging, deploy verification, destructive action prevention) |
| **YouTube** | Episode 9 |

---

## UTILITIES

### `/wrap`

| Aspect | Detail |
|--------|--------|
| **Role** | Graceful session close. Captures state, updates SESH.md + STATUS.md, generates continuation prompt naming the next playbook. |
| **When** | End of any session, any playbook. Context filling up, natural break, need to switch direction. |
| **Output** | Updated SESH.md (Status: CONTINUING), updated STATUS.md, ready-to-paste continuation prompt. |
| **Key behavior** | Continuation prompt includes: which playbook to re-enter, what mode, where work stopped, what's next. Business owner pastes it into next session and picks up immediately. Can point to a DIFFERENT playbook than the one that just ran (e.g., /test wraps and points to /build FIX). |
| **Reference** | Existing wrap skill |

### `/status`

| Aspect | Detail |
|--------|--------|
| **Role** | "Where am I?" Reads SESH.md + STATUS.md, tells the business owner in plain English: what stage they're at, what's done, what's next, any blockers. |
| **When** | Returning after days/weeks. Forgot where things stand. |
| **Output** | Plain English summary. Suggests which playbook to run next. |
| **Direct entry** | Always works. If no SESH.md, says "No project state found. Start with /app or /discover." |

### `/upgrade`

| Aspect | Detail |
|--------|--------|
| **Role** | Pull latest playbooks from repo. |
| **When** | New playbooks ship, bug fixes, improvements. |
| **Output** | Updated skill files. Reports what changed. |

---

# PHASE 4: DOCUMENTATION

| # | Task | Detail | Deliverable |
|---|------|--------|-------------|
| 4.1 | **GETTING-STARTED.md** | What is Claude Code, how to install, how to subscribe, what a terminal is, how to type a slash command. Written for someone who has never opened a terminal. | `GETTING-STARTED.md` |
| 4.2 | **README.md** | What Playbooks is, who it's for, 30-second install, the skill chain at a glance, link to YouTube. | `README.md` |
| 4.3 | **ETHOS.md** | Philosophy: business owners deserve software leverage without learning to code. Problem-first. Thinking before building. Guard rails, not blank canvases. | `ETHOS.md` |
| 4.4 | **ARCHITECTURE.md** | How playbooks compose. The skill chain. SESH.md as skill-to-skill contract. STATUS.md as human dashboard. Composition, handoff, direct entry, and accumulation specs. Config system. | `ARCHITECTURE.md` |
| 4.5 | **PLAYBOOK-BLUEPRINT.md** | How to build your own playbook. Required files, sections, mode design. The "extend Playbooks" guide. | `PLAYBOOK-BLUEPRINT.md` |
| 4.6 | **SECURITY.md** | Secrets (.env), auth patterns, HTTPS, data privacy. For business owners, not engineers. | `SECURITY.md` |
| 4.7 | **Example walkthrough** | Complete end-to-end: real problem → every playbook → live app. Shows every STATUS.md, every handoff, every /wrap. | `examples/full-walkthrough/` |

---

# PHASE 5: YOUTUBE SERIES

*YouTube is the distribution strategy. Episodes are first-class deliverables, not afterthoughts.*

| # | Episode | Playbook | Content |
|---|---------|-------|---------|
| 5.1 | Ep 0: Getting Started | — | Install Claude Code, terminal basics, first slash command |
| 5.2 | Ep 1: Finding the Real Problem | /discover | "I have an idea" → validated problem. Live demo. |
| 5.3 | Ep 2: Stress-Testing Before You Build | /plan | Premortem, inversion, bottleneck, optionality on a real idea. |
| 5.4 | Ep 3: Setting Up Your Infrastructure | /setup | GitHub, Render, domain, config. Live walkthrough of external steps. |
| 5.5 | Ep 4: Writing the Blueprint | /define | PRD from conversation + assets. |
| 5.6 | Ep 5: Designing What You'll Build | /design | Visual mockups, business owner reacting, iterating. |
| 5.7 | Ep 6: Building Your App (Part 1) | /build BOOTSTRAP | PRD → scaffolded playfield. The project structure explained. |
| 5.8 | Ep 7: Building Your App (Part 2) | /build BUILD | Multi-session building, /wrap, reading STATUS.md. |
| 5.9 | Ep 8: Breaking Your App (On Purpose) | /test | Finding bugs, executive summary, loop to /build FIX. |
| 5.10 | Ep 9: Going Live | /launch | Deploy, verify, celebrate. |
| 5.11 | Ep 10: Build Your Own Playbook | Blueprint | Extending Playbooks with custom playbooks. |
| 5.12 | Episode scripts | All | Full script/outline per episode: beats, demos, viewer takeaway. |

---

# PHASE 6: QUALITY GATE

*Nothing ships without passing these.*

| # | Test | Pass criteria |
|---|------|--------------|
| 6.1 | **Fresh install** | Clone, setup, all commands register. Under 60 seconds. |
| 6.2 | **Front door routing** | /app with no state → starts /discover. With existing state → routes to correct playbook. After /wrap → offers resume. |
| 6.3 | **Full skill chain** | Real project through all 8 playbooks. App is live at the end. SESH.md accumulates correctly. STATUS.md readable at every stage. |
| 6.4 | **Direct entry (every playbook)** | Enter each playbook directly without prior skill chain state. Each handles missing SESH.md gracefully — creates it, tells user what's missing from prior stages, doesn't block. |
| 6.5 | **Non-technical user** | Someone who doesn't code follows Getting Started + YouTube, completes the skill chain. Note every confusion. Fix. |
| 6.6 | **Wrap/re-entry** | /wrap at every stage. Continuation prompt names correct playbook. New session picks up cleanly. Backward re-entry works (test→build→test). |
| 6.7 | **/status at every stage** | Correctly reads state. Plain English. Suggests next playbook. |
| 6.8 | **Platform validation** | /setup and /launch work on Render AND Vercel. |
| 6.9 | **Multi-session /build** | 5+ sessions with /wrap. Context preserved. STATUS.md tracks progress. |
| 6.10 | **/upgrade** | Push update, run /upgrade, new content available. |
| 6.11 | **SESH.md accumulation** | After full skill chain, SESH.md has all 8 playbook sections populated. No section overwrites another. |
| 6.12 | **External steps documentation** | /setup and /design external steps (creating accounts, installing plugins) are clear enough for a non-technical user to follow without help. |

---

# INVENTORY

| Category | Count |
|----------|-------|
| Front door | 1 (/app) |
| Pipeline playbooks | 8 (/discover, /plan, /setup, /define, /design, /build, /test, /launch) |
| Utilities | 3 (/wrap, /status, /upgrade) |
| Documentation | 7 (GETTING-STARTED, README, ETHOS, ARCHITECTURE, PLAYBOOK-BLUEPRINT, SECURITY, example walkthrough) |
| YouTube episodes | 11 + scripts |
| Quality gate tests | 12 |
| **Total** | **42** |

---

# EXECUTION ORDER

```
Phase 1 (Repo Architecture)         ← foundation, everything depends on this
Phase 2 (Meta-Infrastructure)        ← protocols before playbooks
Phase 3 (Pipeline Playbooks + Utilities) ← the product
  Shipping phase A: /app, /discover, /plan, /wrap, /status, /upgrade
  Shipping phase B: /setup, /define, /design
  Shipping phase C: /build, /test, /launch
Phase 4 (Documentation)              ← after playbooks exist
Phase 5 (YouTube)                    ← after docs, references live playbooks
Phase 6 (Quality Gate)               ← runs at each shipping phase, full suite before public
```

---

# WHAT'S CUT FROM V1

| Item | Why | Future? |
|------|-----|---------|
| CFO playbook | Personal finance, not app-building | No — different product |
| PA playbook | Personal assistant, not app-building | No — different product |
| EA playbook | Executive coaching, not app-building | No — different product |
| Animator playbook | Specialist polish. Basic motion baked into /build. | v2 enhancement |
| Capturer playbook | Figma MCP not public, specialist use case | v2 when MCPs public |
| Standalone thinking skills | /plan uses them internally. Not needed as separate commands for v1. | v2 if audience wants them |
| /log skill | VPS dependency, internal tooling | Maybe — if abstracted |
| Coder v1 | Superseded by v2 | No |
| Coolify support | Rossco-specific infra | No |
| Multi-host support | v1 targets Claude Code only | v2 for Codex, Cursor |
