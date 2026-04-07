# Security

How Playbooks handles secrets, authentication, and data privacy. Written for business owners, not security engineers.

## The one rule

**Secrets go in .env, never in code, never in git.**

That's the core of it. Everything below is detail.

## What is a secret?

Anything that would be dangerous if someone else saw it:

- API keys (Render, Stripe, OpenAI, etc.)
- Database passwords
- Encryption keys
- OAuth tokens

These are the keys to your app. If they leak, someone else can access your data, your accounts, or your users' information.

## How /build handles secrets

When /build encounters something that needs a secret (a database connection, a payment processor, an external API), it:

1. Puts a placeholder in a `.env` file in your project root
2. Adds `.env` to `.gitignore` (so it never gets pushed to GitHub)
3. Creates a `.env.example` file showing what variables are needed (without the actual values)
4. Tells you in STATUS.md: "You need to add your Stripe API key to .env"

The `.env` file stays on your machine. It never leaves. When you deploy via /launch, you'll set these same values in your hosting platform's environment settings — /launch walks you through this.

## How /setup handles secrets

/setup will ask you for API keys (GitHub token, Render API key) and store them in your local config at `~/.apb/config.yaml`. This file:

- Lives on YOUR machine only
- Is never committed to any git repo
- Is never shared with anyone
- Is used by /launch to deploy your app

## What gets pushed to GitHub

Your code — everything in your project folder EXCEPT:

- `.env` (your secrets)
- `node_modules/` (downloaded libraries, recreated on deploy)
- Any file listed in `.gitignore`

/build creates the `.gitignore` file automatically with sensible defaults.

## Authentication in your app

If your app needs user logins, /build implements standard authentication patterns:

- Passwords are hashed (never stored in plain text)
- Sessions expire after inactivity
- Failed login attempts are rate-limited
- User data is scoped (users can only see their own data)

These aren't optional features — they're defaults. /build enforces them.

## Data privacy

- User data stays in YOUR database on YOUR hosting account
- Playbooks never transmits your project data to third parties
- The playbooks run in Claude Code on your machine — your conversations with the playbooks are subject to Anthropic's privacy policy
- Your source code is pushed to YOUR GitHub repository (private by default)

## What to do if a secret leaks

If you accidentally commit a secret to git:

1. **Change the secret immediately** — go to the service (Render, Stripe, etc.) and regenerate the API key
2. **Update your .env** with the new key
3. **Update your hosting environment** with the new key
4. The old secret is still in git history. For a private repo, this is low risk. For a public repo, consider using a tool like `git-filter-branch` to remove it.

/build and /launch will warn you if they detect secrets in staged files before committing.
