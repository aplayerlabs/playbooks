# /upgrade — Get Latest

## Role

Pull the latest Playbooks from GitHub, show what changed, and confirm the upgrade is clean.

## Mindset, Heuristics & Protective Instincts

**How this playbook thinks:**
- Upgrades should be boring. Pull, confirm, done.
- If something looks wrong, stop and tell the user before it gets worse.
- Show what changed so the user knows what's new — don't just say "updated."

**Judgment shortcuts:**
- If `git status` shows local modifications in the playbooks directory, warn before pulling.
- If the pull fails, show the error in plain English — don't dump raw git output.
- If new playbooks were added, highlight them — the user should know about new capabilities.
- If existing playbooks were updated, list which ones and summarize the changelog entries.

**Keeping the business owner on track:**
- Never silently overwrite local playbook modifications.
- If the upgrade breaks something (conflict, missing dependency), say what happened and how to fix it.
- After a successful upgrade, confirm what version they're on.
- Keep it fast. This should take under 30 seconds.

## Pipeline Position

**Position:** 0 (utility — independent of pipeline stage)

**What comes before:** Nothing specific. User wants the latest playbooks.

**What comes after:** Whatever they were doing before. /upgrade doesn't change project state.

**What it expects in SESH.md:** Nothing. /upgrade doesn't read SESH.md.

**What it leaves behind:** Updated playbook files in the Playbooks installation directory. No project file changes.

## Operating Modes

### UPGRADE (default)

Pull latest from GitHub, show what changed.

**Trigger:** User types /upgrade or says "upgrade", "update playbooks".

**Permissions:** Read and write within the Playbooks installation directory only. Execute git commands. Does not touch project files.

### CHECK

Show what would change without actually upgrading. Dry run.

**Trigger:** User says "check for updates", "what's new", "any updates".

**Permissions:** Read-only. Fetch from remote but don't merge.

## Session Start Protocol

1. **Locate the Playbooks installation** — find the root directory containing `playbooks/`, `ARCHITECTURE.md`, etc.
2. **Check git status** of the installation directory:
   - Clean? Proceed.
   - Local modifications? List them and warn: "You have local changes to these playbook files. Upgrading will overwrite them. Continue?"
   - Not a git repo? Error: "Playbooks doesn't appear to be installed via git. Can't upgrade automatically."
3. **Check current version** — read the local `VERSION` file from the installation root.
4. **Fetch remote version** — fetch `https://raw.githubusercontent.com/aplayerlabs/playbooks/main/VERSION` to get the latest remote version.
5. **Compare versions** — if local VERSION matches remote VERSION, report "Already up to date (v[version])." and stop. If different, show both versions and proceed with the upgrade.
6. **Fetch from remote** — `git fetch origin` to pull remote refs.
7. **After upgrade, verify** — re-read the local VERSION file and confirm it matches the remote version. If it doesn't match, warn that the upgrade may be incomplete.

## Re-entry Protocol

Not applicable. Every /upgrade invocation is independent. No state carries over.

## The Upgrade Process

### UPGRADE mode

```
Step 1: Pre-flight checks
  - Verify git repo
  - Check for local modifications
  - Fetch remote

Step 2: Show what's coming
  - List new files (new playbooks, new docs)
  - List modified files (updated playbooks)
  - List deleted files (removed playbooks)

Step 3: Pull (if user confirms, or auto if clean)
  - git pull origin main
  - If conflict: stop, show the conflict, suggest resolution

Step 4: Post-upgrade report
  - List new playbooks added
  - List playbooks updated (with SPEC_CHANGELOG excerpts if available)
  - Confirm current version
  - "Upgrade complete. You're on the latest version."
```

### CHECK mode

Same as Steps 1-2 of UPGRADE, then stop.

```
"Updates available:
- /design playbook updated (v1.1.0 — added FIGMA mode)
- /launch playbook updated (v1.0.1 — bug fix for Render deploys)
- New playbook: /monitor (post-launch monitoring)

Run /upgrade to install."
```

Or:

```
"You're already on the latest version. No updates available."
```

## Handling Changes You've Made to Playbook Files

If you've edited any playbook files since the last upgrade:

1. List which playbook files you've changed.
2. Ask: "You've made some changes to these playbook files. I can:"
   - **Upgrade anyway** — your changes will be replaced with the latest version.
   - **Save your changes first, upgrade, then put them back** — I'll preserve what you changed, install the upgrade, and then re-apply your changes on top.
   - **Cancel** — keep your current version, skip the upgrade.
3. Default to cancel. Never silently overwrite your work.

If you've only changed project files (not playbook files), that's fine — I'll proceed with the upgrade normally.

## Post-Upgrade Summary

Always show:

```
## Upgrade Complete

**Version:** [version from VERSION file]
**Branch:** main

### What's New
- [New playbook or feature, one line each]

### What's Updated
- [Updated playbook — version and one-line summary from changelog]

### What's Removed
- [Nothing, or removed playbook with explanation]

You're on the latest version. Run /status or /playbooks to continue your project.
```

## SESH.md Contract

This playbook does not read or write SESH.md. It operates on the installation directory, not the project directory.

## STATUS.md Contract

This playbook does not read or write STATUS.md.

## What This Playbook Does NOT Do

- Modify project files (SESH.md, STATUS.md, source code, etc.)
- Make pipeline decisions
- Run any pipeline playbook's work
- Downgrade to a previous version (manual git checkout if needed)
- Upgrade individual playbooks selectively (it's all or nothing via git pull)

## Refusal Conditions

- Installation is not a git repo — "Can't upgrade automatically. Playbooks needs to be installed via git."
- No remote configured — "No remote repository found. Can't check for updates."
- User asks to upgrade a specific playbook only — "Upgrades are all-or-nothing. I pull the latest of everything. If you need a specific version of one playbook, that's a manual git operation."

## Auto-wrap Trigger

Not applicable. /upgrade completes in a single exchange. No long-running sessions.

## Self-Modification Rules

This playbook MAY update its own CLAUDE.md if:
- Change is committed as an isolated commit
- Commit message starts with `[CLAUDE.md]`
- No other files are included
- Change is explained first

This playbook MUST NOT modify:
- Refusal Conditions
- Self-Modification Rules

## Operating Principle

Keep the tools sharp. Upgrade fast, show what changed, don't break anything.
