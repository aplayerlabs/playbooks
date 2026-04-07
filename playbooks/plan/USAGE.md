# Usage — /plan

## Where it lives

```
~/.claude/skills/playbooks/playbooks/plan/
```

Do not copy skill files into your project. The skill reads your project directory — it lives in the skill pack.

## How to start

### FULL mode (default)

Run the complete stress test — all four thinking tools in sequence.

```
/plan
```

or

```
/plan full
```

The skill runs: Premortem, Inversion, Bottleneck/Mitigation, Optionality. After each tool, it shows you what it found and waits for your acknowledgment before continuing.

### FOCUSED mode

Run specific thinking tools only.

```
/plan focused premortem
```

```
/plan focused inversion bottleneck
```

You can name any combination: `premortem`, `inversion`, `bottleneck`, `optionality`. The playbook still requires the premortem to be completed (now or previously) before it can mark /plan as DONE.

## The four thinking tools

| Tool | What it does | Output |
|------|-------------|--------|
| **Premortem** | Imagines the project has failed 6 months out, works backward to find why | 3-7 critical failures with causal chains |
| **Inversion** | Asks "what guarantees failure?" and flips each into a guardrail | 7-10 anti-patterns with actionable guardrails |
| **Bottleneck** | Converts risks into pre-decided responses with observable triggers | If/then table: when I see X, I do Y |
| **Optionality** | Maps approaches by doors opened/closed, ranked by flexibility-to-cost | Ranked options with recommended approach |

## What goes where

| File | Location | Who creates it |
|------|----------|---------------|
| SESH.md | Your project root | /discover or /plan (if it doesn't exist) |
| STATUS.md | Your project root | /discover or /plan (if it doesn't exist) |
| CLAUDE.md | Playbook folder (not your project) | Already exists |

## What happens during a session

1. /plan reads SESH.md — specifically the `## Problem` section from /discover
2. If no problem exists, it asks you to describe the problem before proceeding
3. Runs the thinking tools (all four in FULL, selected ones in FOCUSED)
4. Presents findings after each tool — you acknowledge before it moves on
5. Synthesises everything into a recommended direction
6. Writes to SESH.md (`## Direction` section) and STATUS.md

## When it's done

/plan finishes when:
- At least the premortem has been completed
- The business owner has acknowledged the top risks
- Findings are written to SESH.md

It will suggest running /setup next to configure infrastructure.

## Re-entry

If you run /plan on a project that already has a direction:
- It shows you the existing risks, guardrails, and approach
- Offers to refine, restart, or skip to /setup
- Defaults to refine

## Running without /discover

/plan works standalone. If there's no SESH.md, it creates one. If there's no validated problem, it asks you about the problem directly — a lightweight version of /discover. It will flag the gap: "I'd recommend running /discover for a thorough problem validation, but I can stress-test what we have."

## How it connects to the pipeline

```
... /discover → [ /plan ] → /setup → ...
```

/plan takes the validated problem from /discover and stress-tests the direction before any infrastructure, requirements, or code work begins. Every downstream playbook can reference the risks and guardrails from /plan.
