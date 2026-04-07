# Progress Signaling

Standard markers that skills write into SESH.md so the skill chain knows what happened, what's next, and whether work is blocked.

## Required signals

Every skill writes these at the bottom of SESH.md when finishing or wrapping a session.

### 1. Status signal

```markdown
## Status: [DONE | CONTINUING | BLOCKED | ERROR]
```

| Status | Meaning | What happens next |
|--------|---------|-------------------|
| DONE | All work for this playbook complete | Pipeline moves to next skill |
| CONTINUING | More work remains | Same playbook continues next session |
| BLOCKED | Cannot proceed without intervention | Pipeline halts, blocker shown |
| ERROR | Unrecoverable error | Pipeline halts, error shown |

### 2. Completed this session

```markdown
### Completed This Session
- [x] TASK-006: Auth flow implementation
- [x] TASK-007: Login component
```

Checkbox format with task ID and description. If no tasks (e.g., /discover, /plan), use plain bullets describing what was accomplished.

### 3. Files changed

```markdown
### Files Changed
- src/lib/auth.ts
- src/components/Login.tsx
- STATUS.md
```

List every file modified. Helps the next skill understand what was touched.

### 4. Next up

```markdown
### Next Up
- TASK-008: Dashboard integration
- BUG-003: Fix session timeout
```

Always present. If nothing remains:

```markdown
### Next Up
None — all work complete. Ready for next skill in pipeline.
```

### 5. Blockers

```markdown
### Blockers
None
```

Or if blocked:

```markdown
### BLOCKED: Need Stripe API key to implement payment flow. Business owner needs to create a Stripe account and add the key to .env.
```

## Detection patterns

Regex patterns for programmatic parsing:

| Signal | Pattern |
|--------|---------|
| Done | `/##\s*Status:\s*DONE/i` |
| Continuing | `/##\s*Status:\s*CONTINUING/i` |
| Blocked | `/###?\s*BLOCKED:\s*(.+)/i` (captures reason) |
| Error | `/##\s*Status:\s*ERROR/i` |
| Completed tasks | `/\[x\]\s*(TASK-\d+\|BUG-\d+)/gi` |
| Files changed | `/###\s*Files\s*Changed\n([\s\S]+?)(?=\n###\|\n---\|\n$)/` |
| Next up | `/###\s*Next\s*Up\n([\s\S]+?)(?=\n###\|\n---\|\n$)/` |

## Done check

A skill's work is considered complete when BOTH conditions are true:

```
isDone = (status === "DONE") AND (no items in "Next Up" or "Next Up" says "None")
```

This dual check prevents false positives where status says DONE but there's still listed work.

Every skill must verify this condition before writing `Status: DONE`. If Next Up has items, use `Status: CONTINUING` instead. The diagnostic skills (/app and /status) must verify this condition when reading `Status: DONE` — if Next Up has items, flag the contradiction.

## Anti-patterns

| Don't say | Why | Say instead |
|-----------|-----|-------------|
| "Nothing left to do" | Ambiguous — could match status detection | "## Status: DONE" |
| "I'm done with this" | No structured marker | "## Status: CONTINUING" (if more work) or "## Status: DONE" |
| "Work complete" (without status) | Not parseable | "## Status: DONE" |
| "Blocked because..." | Wrong format | "### BLOCKED: [reason]" |
| Status in the middle of prose | Not detectable at line start | Status on its own line as a heading |

## SESH.md template

```markdown
# SESH.md — [Project Name]

## Problem
[Written by /discover]

## Direction
[Written by /plan]

## Infrastructure
[Written by /setup]

## Requirements
[Written by /define]

## Design
[Written by /design]

## Build
[Written by /build]

## Testing
[Written by /test]

## Deployment
[Written by /launch]

---

## Session: [YYYY-MM-DD]

**Agent:** [Playbook Name]
**Mode:** [Mode]

## Status: [DONE | CONTINUING | BLOCKED | ERROR]

### Completed This Session
- [x] Description

### Files Changed
- path/to/file

### Next Up
- Description

### Blockers
None
```
