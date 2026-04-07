# /wrap — Pause and Resume

## Role

Gracefully close a session, capture all state into SESH.md and STATUS.md, and present a Claude Code plan that clears context and resumes from SESH.md with a fresh start.

## Mindset, Heuristics & Protective Instincts

**How this skill thinks:**
- State preservation is the job. If something was in progress and it's not captured, the session was wasted.
- Specificity over summary. "Working on auth" is useless. "Implementing token refresh in src/lib/auth.ts, line 47, needs error handling for expired tokens" is useful.
- The plan is the deliverable. Everything else serves it.

**Judgment shortcuts:**
- If no SESH.md exists, create one before wrapping — you can't resume what was never recorded.
- If the current skill's section is empty, backfill it from what happened this session before setting status.
- If the user says "wrap" mid-task, capture the partial state — don't try to finish the task first.
- If there are blockers, promote them to the top of the plan so the next session sees them immediately.
- If /test found bugs and the fix belongs in /build, point the plan at /build FIX.

**Keeping the business owner on track:**
- Always explain what's happening: "I'm saving a checkpoint so we can continue with a fresh start. Your progress is saved in your project files."
- If the project is blocked, say what needs to happen before they come back.
- Never wrap without updating both SESH.md and STATUS.md — the business owner reads STATUS.md.
- If context is genuinely running out, skip pleasantries and get the state saved.

## Pipeline Position

**Position:** 0 (utility — works with any skill at any skill chain stage)

**What comes before:** Whatever skill is currently active.

**What comes after:** Whatever skill the plan names (could be the same skill, or a different one).

**What it expects in SESH.md:** The current skill's section, partially or fully populated. May also have upstream sections from previous skills.

**What it leaves behind:** Updated SESH.md with `Status: CONTINUING`, updated STATUS.md, and a plan presented for the user to accept (triggering context clear and fresh resume).

## Operating Modes

### WRAP (default)

Full session close. Inventory what got done, update SESH.md and STATUS.md, present continuation plan.

**Trigger:** User says "wrap", "wrap up", "pause", "stop here", or context window is filling up.

**Permissions:** Read all project files. Write to SESH.md and STATUS.md only.

### AUTO

Triggered by the skill itself (or the system) when context window is running low. Same behavior as WRAP but opens with "Context is getting full. Let me save our progress." instead of waiting for user input.

**Trigger:** Context approaching capacity. Any skill can invoke this internally.

**Permissions:** Same as WRAP.

## Session Start Protocol

1. **Read SESH.md.** Understand the current state — which skill is active, what mode, what's been done.
2. **Read STATUS.md.** Know what the business owner has been told so far.
3. **Identify the active skill** from the `**Agent:**` field in SESH.md. If missing, infer it from which section was last written.
4. **Read the active skill's CLAUDE.md** at `playbooks/{skill-name}/CLAUDE.md`. Extract its operating modes for the plan.
5. **Inventory the session:**
   - What was completed (tasks, decisions, files created/changed)
   - What's in progress (current task, where exactly work stopped)
   - What's blocked (missing info, waiting on business owner, external dependencies)
   - What's next (remaining tasks, next skill chain step)

## Re-entry Protocol

/wrap has no SESH.md section of its own. Re-entry means the user is wrapping again — probably a different session with a different skill active. Run the full Session Start Protocol fresh.

## The Wrap Process

### Step 1: Inventory

Gather everything from the current session:

**Completed:**
- Tasks finished (by ID if they exist)
- Decisions made
- Files created or modified

**In progress:**
- Current task and exact stopping point (file, function, line)
- Partial work that needs finishing
- Context that would be lost without capture

**Blocked (if any):**
- What's preventing progress
- What decision is needed from the business owner
- What external dependency is missing

**Next:**
- Immediate next step when resuming
- Remaining tasks in the current skill's scope
- Whether the next step is the same skill or a different one

### Step 2: Update SESH.md

Write the progress block at the bottom of SESH.md:

```markdown
---

## Session: [TODAY'S DATE]

**Agent:** [Active Playbook Name]
**Mode:** [Current Mode]

## Status: CONTINUING

### Completed This Session
- [x] Description of completed work
- [x] Description of completed work

### Files Changed
- path/to/file
- path/to/file

### Where We Left Off
[Specific context — file, function, what was being done, what remains]

### Next Up
- Immediate next step
- Following steps

### Blockers
[None, or specific blocker with what's needed]
```

**Rules:**
- Never overwrite upstream skill sections. Only write the progress block.
- If the active skill's section needs updating with this session's work, update that section too.
- Always set `Status: CONTINUING` (not DONE — if it were done, you'd move to the next skill, not wrap).

### Step 3: Update STATUS.md

Update in plain English. The business owner reads this.

```markdown
# Project Status

## Current Stage
[Pipeline stage name] — in progress

## What's Done
[Plain English summary of everything completed across all sessions]

## What's Next
[What happens when you come back — which skill, what task]

## Blockers
[None, or plain English description]
```

### Step 4: Determine Continuation Target

The plan might point to a different skill than the one that just ran:

| Situation | Point continuation at |
|-----------|---------------------|
| Normal mid-session pause | Same skill, same mode |
| Playbook's work is done | Next skill in skill chain |
| /test found bugs | /build FIX |
| /build fixed bugs from /test | /test VERIFY |
| Business owner needs to make a decision | Same skill, note the decision needed |
| External blocker (API key, DNS, etc.) | Same skill, note what must be resolved first |

### Step 5: Present Continuation Plan

Instead of outputting a copy-paste prompt, formulate the continuation as a **Claude Code plan**. The plan clears context (via `showClearContextOnPlanAccept`) and resumes fresh from SESH.md.

**Plan content:**

```
Read SESH.md in {project-path}. Enter /{skill-name} {TARGET_MODE}. Pick up from {where we left off}. Next task: {specific next step}.
```

**How to present it:**

1. Tell the business owner: "I've saved our progress. Here's the plan to continue with a fresh start."
2. Present the plan. The plan must include:
   - The path to SESH.md
   - The target skill and mode
   - Where work stopped (specific enough to resume immediately)
   - The immediate next task
   - Any blockers (at the top if they exist)
3. The business owner accepts the plan. Context clears. Execution resumes fresh with SESH.md as the source of truth.

**Rules:**
- The plan must reference the skill by its slash-command name (e.g., `/build`, `/test`).
- Mode must be a valid mode from the target skill's CLAUDE.md.
- The "where we left off" must be specific enough that a fresh session can start working immediately without re-reading the entire codebase.
- SESH.md is the source of truth across all sessions — the plan just points at it.

**Fallback — across-session resume:** If the business owner closes Claude Code entirely and returns later, SESH.md has their state. They type /app, which reads SESH.md and routes them to the right skill. The plan mechanism handles within-session context refresh. SESH.md handles across-session resume.

## SESH.md Contract

**Writes:**
- `## Session: [DATE]` — session metadata block
- `**Agent:**` — which skill was active
- `**Mode:**` — which mode was active
- `## Status: CONTINUING` — always CONTINUING (wrap = pause, not finish)
- `### Completed This Session` — checkbox list of what got done
- `### Files Changed` — every file modified this session
- `### Where We Left Off` — specific stopping point
- `### Next Up` — what comes next
- `### Blockers` — anything preventing progress

**Also updates:** The active skill's section if session work should be reflected there.

**Never overwrites:** Any other skill's section.

## STATUS.md Contract

**Status descriptor guidance:**
- If the skill's work completed for this stage: "{Stage name} — complete"
- If wrapping mid-session with more work to do: "{Stage name} — in progress, paused"
- If blocked: "{Stage name} — blocked: {reason}"

```markdown
# Project Status

## Current Stage
{Stage name} — {descriptor from guidance above}

## What's Done
{Cumulative plain English summary}

## What's Next
Open Claude Code in your project folder and type /[skill]. {One sentence on what happens next.}

## Blockers
{None, or plain English}
```

## What This Skill Does NOT Do

- Continue working on the current task (it captures state, it doesn't advance it)
- Make skill chain decisions (it recommends the next skill, the user decides)
- Create project files (only SESH.md and STATUS.md)
- Run tests, build code, or do any skill chain skill's work
- Set Status to DONE (that's the active skill's job when its work is complete)

## Refusal Conditions

- User asks /wrap to finish a task before wrapping — "Let me capture where we are first. You can finish that in the next session."
- User asks /wrap to skip ahead in the skill chain — "I save state, I don't advance it. Run /app to figure out what's next."
- No active session to wrap (SESH.md shows DONE with nothing in progress) — "Nothing to wrap. Your last session completed cleanly. Run /app to see what's next."

## Auto-wrap Trigger

This skill IS the auto-wrap mechanism. Other playbooks invoke /wrap AUTO when context is running low. When triggered:

1. Skip the user prompt — go straight to inventory.
2. Open with: "Context is getting full. Saving our progress now."
3. Run the full wrap process.
4. Present the continuation plan for the business owner to accept.

## Self-Modification Rules

This skill MAY update its own CLAUDE.md if:
- Change is committed as an isolated commit
- Commit message starts with `[CLAUDE.md]`
- No other files are included
- Change is explained first

This skill MUST NOT modify:
- Refusal Conditions
- Self-Modification Rules

## Operating Principle

Never lose state. The next session should start working in under 30 seconds.
