# Usage — /setup

## Where it lives

```
~/.claude/skills/playbooks/playbooks/setup/
```

Do not copy skill files into projects. /setup reads from the skill folder and writes to your project directory and `~/.apb/`.

## How to start

### First time ever (INFRA mode)

```
/setup
```

If `~/.apb/config.yaml` doesn't exist, /setup automatically enters INFRA mode. It walks you through:
1. GitHub account (create or verify)
2. Hosting platform (Render recommended)
3. API keys (stored locally, never in git)
4. How secrets work

You'll need to do some steps outside Claude Code (creating accounts, copying keys). /setup tells you exactly what to click and where.

### Per-project setup (PROJECT mode)

```
/setup
```

If `~/.apb/config.yaml` already exists, /setup enters PROJECT mode for the current directory. It creates:
- `deploy.json` — branch-to-environment mapping
- `.env.example` — placeholder for secrets (populated later by /build)
- `.gitignore` — with `.env` excluded
- Git repo initialized and connected to GitHub (if not already)

### Re-entry (already set up)

```
/setup
```

If infrastructure is already configured for this project, /setup offers:
- **Refine** — update config, change platform, add deploy targets
- **Restart** — clear config and start over
- **Skip** — move to /define

## What goes where

| File | Location | Committed to git? |
|------|----------|-------------------|
| `config.yaml` | `~/.apb/` | No — your machine only |
| `deploy.json` | Project root | Yes — no secrets in it |
| `.env` | Project root | No — gitignored |
| `.env.example` | Project root | Yes — shows what's needed |
| `.gitignore` | Project root | Yes |

## In the pipeline

```
/discover → /plan → [ /setup ] → /define → /design → /build → /test → /launch
```

/setup can also be entered directly. If you just want hosting configured, type `/setup` in any project directory. It creates SESH.md if missing and proceeds.

## External steps

Some steps happen outside Claude Code. /setup provides exact instructions for:
- Creating a GitHub account
- Creating a Render account
- Generating API keys
- Connecting your repo to hosting

You do the clicking, /setup does the configuring.
