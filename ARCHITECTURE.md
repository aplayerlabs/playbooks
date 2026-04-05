# Architecture

How A Player Brains works under the hood.

## Overview

A **brain chain** is a coordinated sequence of specialist brains where each brain's output feeds the next. The A Player Brains chain runs from /discover through /launch — eight brains in order, connected through SESH.md.

A Player Brains is a set of specialist AI brains, each implemented as a Claude Code skill (slash command). They share state through two files in the project directory and follow a linear brain chain from problem discovery to deployment.

```
/aplayerbrains → /discover → /plan → /setup → /define → /design → /build → /test → /launch
```

Three utility skills support the brain chain: /wrap (pause and resume), /status (where am I?), /upgrade (get latest brains).

## The two files

Every brain reads and writes two files in the project root:

### SESH.md (brain-to-brain contract)

Structured handoff data. Each brain owns a section. Sections accumulate as the project moves through the brain chain — brains add to SESH.md, they never overwrite other brains' sections.

```
## Problem          ← written by /discover
## Direction        ← written by /plan
## Infrastructure   ← written by /setup
## Requirements     ← written by /define
## Design           ← written by /design
## Build            ← written by /build
## Testing          ← written by /test
## Deployment       ← written by /launch
```

Every brain also writes a standard progress block at the bottom:

```
## Status: [DONE | CONTINUING | BLOCKED | ERROR]

### Completed This Session
### Files Changed
### Next Up
### Blockers
```

### STATUS.md (human-readable dashboard)

Plain English. The business owner reads this. Every brain updates it with what happened and what's next. No technical jargon. No code references.

**SESH.md is the source of truth for brain chain state.** STATUS.md is a human-readable projection of that state. When they conflict, SESH.md wins. If /status or /aplayerbrains detects a discrepancy, it flags: "STATUS.md says [X] but SESH.md says [Y]. SESH.md is the authority — STATUS.md may need updating."

Later stages add structured fields:
- Ship readiness: RED / YELLOW / GREEN (added by /test)
- Version and URL (added by /launch)

## How brains hand off

Each brain follows the same protocol:

**On start:**
1. Check if SESH.md exists
2. If yes: read it, understand current state, continue
3. If no: create it (the brain is the first touch on this project)
4. Read STATUS.md if it exists
5. Orient the user: "Here's where we are. Here's what I'll do."

**On finish (or /wrap):**
1. Update SESH.md — own section + progress block
2. Update STATUS.md — plain English summary
3. If wrapping: generate continuation prompt naming the next brain

## Direct entry

Any brain can be entered directly without going through the full brain chain. When a brain is entered directly:

0. If SESH.md does not exist, check git history (`git show HEAD:SESH.md`). If recoverable, restore it and notify: "SESH.md was missing but I recovered it from your last commit." If not recoverable (e.g., initial commit with no prior SESH.md, detached HEAD, or repository has no commits yet) -- proceed with creating a fresh SESH.md and backfilling from project artifacts.
1. Check if SESH.md exists. If not, create it with all section headers.
2. Look around the project for anything upstream brains would have produced — PRD, deploy.json, package.json, existing docs, running app, design files. Read what's there.
3. Backfill SESH.md from whatever exists. If there's a PRD, extract the problem statement into `## Problem`, the scope into `## Requirements`, any design notes into `## Design`. If there's a deploy.json, populate `## Infrastructure`. If there's a running app, note it in `## Build`.
4. Flag what's genuinely missing. Tell the user honestly: "I don't have a validated direction from /plan. That means we haven't stress-tested this idea yet. I'd recommend running /plan, but I can work with what we have."
5. Do NOT block. Adapt and proceed.

The pattern is: **read the room, backfill SESH.md from whatever exists, flag the gaps, keep moving.**

This means the brain chain is the recommended path, but not the only path. Someone who already has a PRD can type /build directly — the brain reads the PRD, backfills SESH.md, and starts building. Someone who just wants to test an existing app can type /test directly — the brain looks at the codebase and proceeds.

## Re-entry to a completed brain

When a brain is invoked on a project where it has already run (its SESH.md section is populated):

1. Acknowledge the existing work: "You already have a validated problem: [X]."
2. Offer three options: **refine** (iterate on what's there), **restart** (clear and redo), or **skip** (move to the next brain).
3. Default to refine. Never silently overwrite prior work.

## SESH.md is git-tracked

SESH.md must be committed to git. This means:
- If the business owner accidentally deletes it, `git checkout SESH.md` recovers it
- The full history of brain chain decisions is in version control
- /build commits SESH.md alongside code changes

STATUS.md is also git-tracked for the same reasons.

## One project per directory

Each project directory has exactly one SESH.md and one STATUS.md. Do not run two projects in the same folder. If the business owner wants a second project, they create a new directory.

## Concurrent session protection

On session start, check for `.sesh.lock` in the project root. If it exists and was created within the last 2 hours, warn: "Another session may be active on this project. Running two sessions simultaneously can corrupt SESH.md. Continue anyway?" On session end or /wrap, remove `.sesh.lock`. If `.sesh.lock` is older than 2 hours, treat it as stale (from a crashed session) and proceed.

## Auto-wrap

When context window is running low, the brain should proactively trigger /wrap behavior rather than waiting for the user to notice. This is especially critical for /build which runs long sessions. The brain:

1. Detects context is filling up
2. Tells the user: "Context is getting full. Let me save our progress."
3. Runs the /wrap protocol (update SESH.md, update STATUS.md, generate continuation prompt)

This is a safety net. The user can still /wrap manually at any time.

## Config is always fresh

Brains always read `~/.apb/config.yaml` and `deploy.json` fresh at session start. Never cache config from a previous session. If the business owner re-runs /setup to change hosting platforms, every downstream brain picks up the change automatically.

## /wrap and re-entry

/wrap is the universal pause button. It:

1. Captures what was done this session
2. Updates SESH.md (Status: CONTINUING)
3. Updates STATUS.md
4. Generates a continuation prompt ready to paste into the next session

The continuation prompt includes:
- Which brain to re-enter
- What mode
- Where work stopped
- What's next

**Backward movement:** /wrap can point to a different brain than the one that just ran. If /test finds bugs, /wrap generates a continuation prompt pointing to /build FIX. After /build fixes them, /wrap points back to /test VERIFY. The brain chain stays linear — /wrap handles direction changes.

**SESH.md always takes precedence over continuation context.** If a continuation references state that doesn't match current SESH.md, follow SESH.md.

## SESH.md accumulation

Each brain owns its section and only writes to it. This prevents overwrites as the document travels the brain chain.

| Brain | Section | What it writes |
|-------|---------|----------------|
| /discover | `## Problem` | Validated problem statement — who has it, what they do today, why it hurts |
| /plan | `## Direction` | Top risks, pre-decided responses, guardrails, recommended approach |
| /setup | `## Infrastructure` | Platform choice, repo URL, branch mapping, deploy config location |
| /define | `## Requirements` | PRD location, scope summary (in/out), stage breakdown |
| /design | `## Design` | Design decisions, mockup/Figma locations, visual direction confirmed |
| /build | `## Build` | Architecture decisions, current task, progress, files changed |
| /test | `## Testing` | Coverage summary, bugs found, severity breakdown, ship readiness |
| /launch | `## Deployment` | Version, live URL, deploy log location, rollback command |

## Progress signaling

The standard progress block at the bottom of SESH.md uses structured markers that can be parsed programmatically:

**Status signal:**
```markdown
## Status: DONE
```
Valid values: `DONE`, `CONTINUING`, `BLOCKED`, `ERROR`

**Blocker signal:**
```markdown
### BLOCKED: [reason]
```

**Detection patterns (regex):**
```
Done:       /##\s*Status:\s*DONE/i
Continuing: /##\s*Status:\s*CONTINUING/i
Blocked:    /###?\s*BLOCKED:\s*(.+)/i
Error:      /##\s*Status:\s*ERROR/i
Completed:  /\[x\]\s*(TASK-\d+|BUG-\d+)/gi
```

See [PROGRESS-SIGNALING.md](PROGRESS-SIGNALING.md) for the full spec.

## Config system

User-level configuration lives at `~/.apb/config.yaml`. Created by /setup.

```yaml
github_user: "username"
platform: "render"           # render | vercel
default_branch: "main"
staging_branch: "staging"
```

Brains that need infrastructure config (/define, /design, /build, /launch) read from this file. Brains that run before /setup (/discover, /plan) don't need it.

Project-level deployment config lives at `deploy.json` in the project root. Also created by /setup.

```json
{
  "targets": {
    "staging": {
      "branch": "staging",
      "url": "https://staging.example.com"
    },
    "production": {
      "branch": "main",
      "url": "https://example.com",
      "confirm": true
    }
  }
}
```

## Composition

Brains can use other brains' thinking internally without exposing separate commands:

- /plan runs premortem, inversion, bottleneck analysis, and optionality mapping as part of its process. These are thinking tools embedded in /plan, not standalone commands.
- /aplayerbrains reads SESH.md and routes to the appropriate brain. It's a router, not a worker — it doesn't modify project state.
- /wrap reads the current brain's spec to list available modes in the continuation prompt.

## The front door: /aplayerbrains

The one command the business owner needs to remember.

**No SESH.md found:** "Looks like a new project. Let's start by finding the real problem." Routes to /discover.

**SESH.md exists:** Reads current state, determines brain chain stage, tells user where they are, suggests next brain.

**After /wrap:** Reads continuation state, offers to resume where they left off.

## The Upgrade Protocol

Building a brain is half the job. The other half is getting humans to actually use it. The Upgrade Protocol is the framework for enrollment — turning a delivered brain into an adopted tool.

When a business owner's app ships and their team starts using it, this is how adoption happens. It's also relevant for community brain builders — think about adoption, not just design.

### Four preconditions

Before showing anyone a brain, verify these four conditions. If any are missing, enrollment will fail:

1. **Why** — The human understands why the brain is better than what they do today
2. **How** — They can see how to use it without confusion
3. **Confidence** — They feel confident they can execute
4. **Worth** — They believe it's worth the effort

### Five-part enrollment structure

1. **Name the purpose** — Frame as transformation, not tool training. "This frees you to do the work that matters."
2. **Expose the struggles** — Name the frustrations, mistakes, and missed opportunities BEFORE showing the brain. The human needs to feel the pain first.
3. **Show the idea** — Create an irreversible mental shift. "You think [X] creates results, but it's actually [Y]."
4. **Map the steps** — Only NOW show how. Dead simple. What to have open, first thing to do, what happens next, what "done" looks like.
5. **Lock the action** — Remind of trigger, big idea, and how to flag issues.

### First-week cadence

| Day | Activity |
|-----|----------|
| 1 | Walkthrough |
| 2 | Check-in |
| 3 | Observe |
| 4 | Refine |
| 5 | Feedback |
| 6-7 | Space |
| End of week | Debrief |

### The key principle

"You're not being replaced. You're being upgraded."

Then follow through — give harder, more interesting work. Not more admin. If the team sees the brain handling the boring stuff and their job getting better, adoption is permanent. If they see the brain as one more thing to learn with no payoff, it dies in a week.

## Brain file structure

Every brain follows the same file structure (see [BRAIN-BLUEPRINT.md](BRAIN-BLUEPRINT.md) for full spec):

```
brains/{brain-name}/
  SKILL.md              ← Frontmatter + skill registration
  CLAUDE.md             ← Operating contract (the brain's rules)
  USAGE.md              ← How to invoke, modes, examples
  SPEC_CHANGELOG.md     ← Version history of the brain spec
  SPEC_DECISIONS.md     ← Why the rules exist
  playbooks/            ← Optional. Reusable patterns.
```

## Security

See [SECURITY.md](SECURITY.md) for how secrets, auth, and data privacy are handled.

## Skill file format

See [SKILL-FORMAT.md](SKILL-FORMAT.md) for the YAML frontmatter spec that registers brains as slash commands.
