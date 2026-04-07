# Usage — /test

## Where it lives

```
~/.claude/skills/playbooks/playbooks/test/
```

Do not copy skill files into your project. The skill reads your project directory — it lives in the skill pack.

## How to start

### HUNT mode (default)

Go deep. Find everything. No time limit.

```
/test
```

or

```
/test hunt
```

The skill will read your project, understand what was built, and systematically try to break it.

### VERIFY mode

Targeted check. "Does this specific thing work?"

```
/test verify login flow
```

```
/test verify BUG-003 fix
```

Fast and focused. Binary outcome: works or doesn't.

### REGRESS mode

Check that old bugs stayed fixed.

```
/test regress
```

Reads `bugs/resolved.md` and tries to reproduce each one.

### EDGE mode

Boundary conditions, abuse cases, security.

```
/test edge
```

Thinks like an attacker. Tries weird inputs, broken sequences, malicious payloads.

## What goes where

| File | Location | Who creates it |
|------|----------|---------------|
| SESH.md | Your project root | /test (if it doesn't exist) |
| STATUS.md | Your project root | /test (if it doesn't exist) |
| bugs/open.md | Your project root | /test |
| bugs/resolved.md | Your project root | /build (when bugs are fixed) |
| CLAUDE.md | Playbook folder (not your project) | Already exists |

## What happens during a session

1. /test reads SESH.md — sees what /build did, what's been tested before
2. Reads requirements (PRD, requirements/) — understands what "correct" means
3. Reads existing bugs — avoids re-reporting known issues
4. Systematically tests based on the mode you chose
5. Documents every bug found in `bugs/open.md`
6. Writes an executive summary with ship readiness (GREEN / YELLOW / RED)
7. Updates SESH.md (`## Testing`) and STATUS.md

## Reading the executive summary

After every test session, STATUS.md tells you in plain English:

- **Ship readiness:** GREEN (go), YELLOW (your call), or RED (don't ship)
- **Bug count** by severity
- **Top blocker** — the worst issue, explained simply
- **Ready to ship: Yes / No** with a one-sentence reason

## The test-fix loop

When /test finds bugs:

```
/test → finds bugs → /wrap (points to /build FIX)
    ↓
/build FIX → fixes bugs → /wrap (points to /test VERIFY)
    ↓
/test verify → confirms fixes → updates ship readiness
```

Repeat until GREEN (or YELLOW with your approval).

## Re-entry

If you run /test on a project that already has test results:
- It shows you existing results and coverage
- Offers to continue (test untested areas), restart (fresh), or skip to /launch
- Defaults to continue

## How it connects to the pipeline

```
... → /build → [ /test ] → /launch
```

/test is the quality gate. It tells you the truth about whether your app is ready for users. /launch won't deploy without /test having run.
