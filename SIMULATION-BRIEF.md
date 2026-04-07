# Simulation Brief: Full End-to-End Run (With Real Code)

## Your job

Run a complete end-to-end simulation of the Playbooks skill chain. Play both roles:

- **THE PLAYBOOK:** Follow each playbook's CLAUDE.md spec exactly. Read the file before each stage.
- **THE BUSINESS OWNER (Marcus):** Give realistic, non-technical responses.

**CRITICAL: This simulation produces REAL CODE.** The /build stage writes a real, working application. When the simulation finishes, the app must run on localhost. Paper-only simulation is worthless.

## The test case

**Marcus** runs a small personal training studio with 6 trainers. He manages client bookings through a shared Google Calendar, but it's a mess:

1. Clients text or DM him on Instagram asking to book a session
2. He checks the Google Calendar to see which trainers are free
3. He manually adds the booking to the calendar, sometimes forgetting to include the trainer's name or the session type
4. Clients cancel last-minute (2-4 times per week) and he has to scramble to fill the slot or notify the trainer
5. He has no idea which trainers are fully booked vs underutilized — he just "feels" it
6. End of month, he manually counts sessions per trainer from the calendar to calculate their pay

He wants a simple booking tool where clients can see available slots and book directly, trainers can see their schedule, and he can track session counts for payroll.

**Marcus's personality:** Energetic, impatient, uses his phone for everything. He's 28, runs the business with just himself and a part-time admin. He's not technical but he's quick to learn apps — he uses Canva, Google Sheets, and Instagram daily. He's tried Mindbody and Acuity but they were overkill and too expensive ($60-100/month) for 6 trainers.

## Project setup

Create the project at: `./simulation/` (relative to this file's location — i.e., inside the playbooks repo)

Add `simulation/` to `.gitignore` if it isn't already — simulation output should not pollute the skill chain repo.

## The simulation sequence

Run every playbook in order. At each stage, produce ALL of the following:

### For each playbook:

**1. Read the spec**
Read `playbooks/{name}/CLAUDE.md` at the path relative to this file's repo root.

**2. Show the conversation**
Write out the dialogue between the playbook and Marcus. The playbook follows its spec. Marcus responds realistically. Keep it tight — don't pad conversations. Get to the point.

**3. Show SESH.md updates**
After each playbook completes, show the EXACT content that would be written to the playbook's owned section in SESH.md. Field by field. Use the SESH.md contract from each playbook's CLAUDE.md.

**4. Show STATUS.md updates**
Show the EXACT plain-English STATUS.md content. Use the STATUS.md contract from each playbook's CLAUDE.md.

**5. Write real files**
From /setup onward, create real project files. From /build onward, write real application code. The app must be runnable on localhost when the simulation finishes.

**6. Mark gaps**
If at any point the spec is ambiguous, missing guidance, contradicts itself, or would produce a bad experience, mark it inline as:
```
[GAP-XX: description of the issue — what the spec says, what's needed, severity]
```

## Stage-by-stage verification checklist

### Stage 1: /app (router)
Spec: `playbooks/playbooks/CLAUDE.md`
- No SESH.md exists → should detect fresh project
- Should route to /discover
- Should speak plain English, no jargon
- Should NOT modify any files (read-only router)

### Stage 2: /discover (validate problem)
Spec: `playbooks/discover/CLAUDE.md`
- Should create SESH.md and STATUS.md (first playbook to do so)
- Should ask structured questions (Question Engine phases 1-2: Reality + Outcome)
- Should run F-I-C diagnostic (Frustration → Impact → Cause)
- Should classify: Bottleneck vs Strategy Gap
- Should categorise: Drag / Friction / Inertia
- SESH.md ## Problem must have ALL fields: Who, Problem, Current approach, Why it hurts, Success looks like, Evidence (Observed/Assumed), Type (Bottleneck/Strategy Gap), Category (Drag/Friction/Inertia), Validated (Yes/No)
- Guard rail: cannot mark Status: DONE if Validated: No
- STATUS.md must be plain English a business owner can read

### Stage 3: /plan (stress-test direction)
Spec: `playbooks/plan/CLAUDE.md`
- Should read ## Problem from SESH.md
- Should detect Type: Bottleneck and adapt (fast build, immediate ROI framing)
- Must run 4 thinking tools in order:
  1. **Premortem:** 3-7 critical failures with causal chains. Make failure vivid.
  2. **Inversion:** Anti-patterns flipped to guardrails. Two-column table.
  3. **Bottleneck/Mitigation:** If/then trigger-response table. Observable triggers.
  4. **Optionality:** Options with doors opened/closed, cost, ranking.
- Guard rail: cannot exit without premortem completed
- Business owner must acknowledge top risks
- SESH.md ## Direction must have: risks, guardrails, recommended approach, pre-decided responses

### Stage 4: /setup (infrastructure)
Spec: `playbooks/setup/CLAUDE.md`
- **SIMULATED external steps** — Marcus "has" a GitHub account (username: `marcusfitpt`) and a Render account. Don't actually create accounts. Use placeholder credentials.
- Cost guidance must be honest: mention free tier limits, auto-sleep, Render DB 90-day expiry, expected costs
- Teach .env pattern
- **CREATE REAL FILES:** deploy.json, .env.example, .gitignore in the simulation/ project directory
- Show exact config content
- SESH.md ## Infrastructure must have: platform, repo, branches, config path, deploy config path, secrets pattern

### Stage 5: /define (write PRD)
Spec: `playbooks/define/CLAUDE.md`
- Run Question Engine phases 3-8: Solution, Scope, Data, Experience, Deployment, Constraints
- Scope gate: Marcus must confirm in/out tables before PRD is written
- If >7 features in Stage 0, flag scope size
- **WRITE REAL PRD** to simulation/prd/prd.md
- SESH.md ## Requirements must have: PRD location, scope summary, in-scope features, deferred features, data source, deploy target, Stage 0 exit criteria

### Stage 6: /design (visual mockups)
Spec: `playbooks/design/CLAUDE.md`
- Marcus doesn't use Figma → HTML MOCKUP mode
- Identify 2-4 key screens from PRD
- **WRITE REAL HTML MOCKUPS** to simulation/mockups/
- Design system: colors, typography, spacing, border radius — documented in SESH.md and in _styles.css
- Marcus approves the direction
- SESH.md ## Design must have: path, design system, screens, design decisions, approval status

### Stage 7: /build (build the app)
Spec: `playbooks/build/CLAUDE.md`
- **THIS IS THE CRITICAL STAGE. WRITE REAL CODE.**
- BOOTSTRAP: pick stack — keep it simple. Vite + React + Tailwind is fine. No database required — localStorage or in-memory state is acceptable for a simulation. Keep scope tight.
- Scaffold the playfield: tasks/, bugs/, requirements/, roadmap/, docs/
- Create Stage 0 tasks from PRD
- **BUILD ALL TASKS.** Write real components, real pages, real logic.
- The app must start with `npm run dev` (or equivalent) and work on localhost.
- SESH.md ## Build must have: architecture decisions, bootstrap status, task progress

### Stage 8: /test (break the app)
Spec: `playbooks/test/CLAUDE.md`
- HUNT mode: test the running app
- File 3-5 realistic bugs in simulation/bugs/open.md using the UNIFIED bug format
- Executive summary with ship readiness (RED/YELLOW/GREEN)
- Coverage tracking
- SESH.md ## Testing must have: last tested, mode, ship readiness, bug summary, coverage, key findings

### Stage 9: /launch (deploy)
Spec: `playbooks/launch/CLAUDE.md`
- PREFLIGHT: evaluate all 12 checklist items (PASS/WARN/FAIL each one)
- **SIMULATE the deploy** — don't actually push to Render. But do create the shipped/ folder, release notes, deploy log, version tag.
- Marcus gives "ship it" confirmation
- SESH.md ## Deployment must have: version, simulated URL, deploy log location, rollback command
- STATUS.md final: plain English, "v1.0.0 is LIVE" with rollback instructions

## After all 9 stages

### Verify the app runs

```bash
cd simulation/
npm install
npm run dev
```

The app MUST start and be usable in a browser. If it doesn't, the simulation failed.

### Produce a GAP REPORT

Collect all [GAP-XX] markers and organize:

| # | Stage | Gap | Severity | Description |
|---|-------|-----|----------|-------------|
| 1 | /discover | ... | Critical/Medium/Low | ... |

### Verify completeness

Check the final SESH.md:
- [ ] ## Problem populated
- [ ] ## Direction populated
- [ ] ## Infrastructure populated
- [ ] ## Requirements populated
- [ ] ## Design populated
- [ ] ## Build populated
- [ ] ## Testing populated
- [ ] ## Deployment populated
- [ ] All 8 sections owned by the correct playbook
- [ ] No section overwrote another

Check STATUS.md:
- [ ] Current Stage: Deployed / Live
- [ ] Plain English throughout
- [ ] A non-technical person could read this and understand the project state

Check the app:
- [ ] `npm install && npm run dev` succeeds
- [ ] App loads in browser
- [ ] Core flow works (book a session, view schedule, see trainer utilization)

## Important notes

- **REAL CODE.** The /build stage writes a real application. Not pseudocode, not descriptions of what would be built. Real files, real components, real logic.
- Read each playbook's CLAUDE.md BEFORE simulating that stage. Don't improvise — follow the spec.
- Marcus is non-technical. If the playbook would say something he wouldn't understand, that's a gap.
- The skill chain terminology should be used throughout (not "pipeline").
- If a playbook's spec is missing guidance for a situation that arises, mark it as a gap even if you can improvise an answer.
- Keep conversations tight. Don't pad dialogue. Get to the point fast — the code is what matters.
- For /build, prioritize working code over comprehensive documentation. Ship something real.
- localStorage is fine for data persistence. No need for Supabase or any external database in the simulation.
