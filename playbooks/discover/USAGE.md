# Usage — /discover

## Where it lives

```
~/.claude/skills/apb/playbooks/discover/
```

Do not copy skill files into your project. The skill reads your project directory — it lives in the skill pack.

## How to start

### PROBLEM mode (default)

You have a pain point. Something hurts, something's slow, something's broken.

```
/discover
```

or

```
/discover problem
```

The skill will ask you structured questions about the problem: who has it, what they do today, why it hurts, how often.

### IDEA mode

You have a concept but haven't pinned down the problem yet.

```
/discover idea
```

The skill will work backward from your idea to find the problem underneath. "Who would use this, and what problem does it solve for them?"

## What goes where

| File | Location | Who creates it |
|------|----------|---------------|
| SESH.md | Your project root | /discover (if it doesn't exist) |
| STATUS.md | Your project root | /discover (if it doesn't exist) |
| CLAUDE.md | Playbook folder (not your project) | Already exists |
| Reference assets | `reference/` in your project | You |

## What happens during a session

1. /discover checks for SESH.md and STATUS.md — creates them if missing
2. Reads any files in your project for context (reference materials, screenshots, spreadsheets)
3. Asks structured questions — one at a time, with options
4. Confirms understanding before moving forward
5. Assembles a validated problem statement
6. Writes to SESH.md (`## Problem` section) and STATUS.md

## Providing assets

Drop files into a `reference/` folder in your project root. /discover reads:
- Spreadsheets (.xlsx, .csv)
- PDFs
- Screenshots (.png, .jpg)
- Any text files

The skill reads these directly — you don't need to explain what's in them.

## When it's done

/discover finishes when you've confirmed a validated problem statement. It will suggest running /plan next to stress-test the direction.

## Re-entry

If you run /discover on a project that already has a validated problem:
- It shows you the existing problem statement
- Offers to refine, restart, or skip to /plan
- Defaults to refine

## How it connects to the pipeline

```
/app → [ /discover ] → /plan → ...
```

/discover is the first skill. It creates the project state files (SESH.md, STATUS.md) that every other skill reads and builds on.
