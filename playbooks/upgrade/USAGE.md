# /upgrade — Usage

## Where it lives

```
playbooks/upgrade/
  SKILL.md
  CLAUDE.md
  USAGE.md
  SPEC_CHANGELOG.md
  SPEC_DECISIONS.md
```

Do not copy these files into your project. The skill system reads them from the playbooks directory.

## How to invoke

```
/upgrade
```

Or say: "upgrade", "update skills"

## Modes

### UPGRADE (default)

Pull the latest version and show what changed.

```
/upgrade
```

Output:
```
Upgrade Complete

Version: a1b2c3d
Branch: main

What's New:
- /monitor playbook added (post-launch monitoring)

What's Updated:
- /build v1.2.0 — added HOTFIX mode for production patches
- /design v1.1.0 — added FIGMA mode for Figma integration

You're on the latest version. Run /status or /app to continue your project.
```

### CHECK

See what's available without upgrading.

```
/upgrade CHECK
```

Or say: "check for updates", "what's new", "any updates"

Output:
```
Updates available:
- /build updated (v1.2.0 — added HOTFIX mode)
- New skill: /monitor

Run /upgrade to install.
```

Or:
```
You're already on the latest version. No updates available.
```

## Local modifications warning

If you've edited any skill files locally, /upgrade will warn you before overwriting:

```
You've modified these skill files locally:
- playbooks/build/CLAUDE.md

Options:
1. Upgrade anyway (overwrites your changes)
2. Stash and upgrade (saves changes, upgrades, you re-apply manually)
3. Cancel (keep your version)
```

## How it works with the pipeline

/upgrade doesn't affect your project at all. It updates the skill files themselves — the instructions that each skill follows. Your SESH.md, STATUS.md, and all project files stay untouched.

Think of it like updating your tools, not your work.
