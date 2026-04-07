# Usage — /launch

## Where it lives

```
~/.claude/skills/apb/playbooks/launch/
```

Do not copy skill files into your project. The skill reads your project directory — it lives in the skill pack.

## How to start

### SHIP mode (default)

Full deployment: preflight checks, release notes, version bump, deploy, verify.

```
/launch
```

or

```
/launch ship
```

The skill runs preflight automatically, then walks you through deployment. You'll be asked to confirm before anything goes live.

### PREFLIGHT mode

Check if everything is ready, without deploying.

```
/launch preflight
```

Runs the full checklist and reports PASS / WARN / FAIL for each item. Nothing is pushed or changed.

### NOTES mode

Write release notes only, no deploy.

```
/launch notes
```

Useful for drafting what's going out before you're ready to deploy.

## What goes where

| File | Location | Who creates it |
|------|----------|---------------|
| SESH.md | Your project root | /launch (if it doesn't exist) |
| STATUS.md | Your project root | /launch (if it doesn't exist) |
| deploy.json | Your project root | /setup |
| ~/.apb/config.yaml | Your home directory | /setup |
| shipped/{version}/ | Your project root | /launch |
| CHANGELOG.md | Your project root | /launch (updates) |
| CLAUDE.md | Playbook folder (not your project) | Already exists |

## What happens during SHIP

1. /launch reads your project state — what was built, what was tested, deployment config
2. Runs the preflight checklist — any failures stop the process
3. Determines the version number (or asks you)
4. Writes release notes
5. Bumps the version in package.json and CHANGELOG.md
6. Asks you to confirm: "Type 'ship it' to deploy"
7. Pushes to your hosting branch
8. Waits for the deploy to complete
9. Checks that your URL is live and responding
10. Reports success or failure in plain English

## The preflight checklist

Before deploying, /launch verifies:

1. deploy.json exists and is valid
2. Hosting platform is connected and responding
3. No critical or high bugs in bugs/open.md
4. /test has run (Testing section in SESH.md)
5. Ship readiness is GREEN or YELLOW (not RED)
6. All code is committed
7. Version number is set
8. You've confirmed deployment (during SHIP)

Any FAIL stops the deploy. Any WARN asks for your acknowledgment.

## After deployment

STATUS.md tells you in plain English:
- Your version number and live URL
- What's in the release
- What to do if something goes wrong (rollback instructions)

## Re-entry

If you run /launch on a project that's already been deployed:
- It shows you the current deployed version and URL
- Offers to deploy a new release, redeploy the same version, or skip
- Defaults to new release

## How it connects to the pipeline

```
... → /test → [ /launch ]
```

/launch is the final playbook. After it runs, your app is live. To make changes later, start a new cycle through the pipeline with /app.
