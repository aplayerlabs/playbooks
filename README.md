# Playbooks

Encode what your experts know into AI skill chains that anyone can run.

Playbooks is a framework for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) that turns expert knowledge — the pattern recognition, judgment calls, and hard-won shortcuts that live in people's heads — into repeatable, structured AI workflows. Each playbook is a chain of specialist skills that guide a user through a domain, step by step, with the thinking built in.

## The structure

Every playbook follows the same architecture:

- **Playbook** — the complete workflow. One playbook per domain. It threads skills together into a coherent process.
- **Skills** — the specialist steps in the chain. Each skill is a slash command with a single job: discover, plan, build, test, etc.
- **Plays** — the moves within each skill. Modes, protocols, heuristics, decision trees — the expertise that makes the skill smart, not just procedural.
- **Playfield** — the project folder structure that every skill reads from and writes to. The shared surface where all work happens.

## What ships with this repo

A complete **problem-to-app playbook** — eight skills that take a non-technical business owner from "I have a problem" to "my app is live." This is the proof of concept. It demonstrates the full architecture: skill chaining, handoff state, session persistence, the playfield, and the plays that encode real builder expertise.

### The included skill chain

```
/app    Start here. Reads your project state and routes you to the right skill.
      |
/discover         Validate your problem (or refine your idea into one)
      |
/plan             Stress-test the direction before building anything
      |
/setup            Set up GitHub, hosting, and deployment config
      |
/define           Write a sharp product requirements document
      |
/design           See what your app will look like before code is written
      |
/build            Build the application (runs across multiple sessions)
      |
/test             Break it on purpose. Find every bug.
      |
/launch           Deploy. Your app is live.
```

Utilities: `/wrap` (pause and resume), `/status` (where am I?), `/upgrade` (get latest).

This is one playbook. Your playbook would have different skills, in a different order, for a different domain.

## Build your own

The included playbook is an example. The real product is the framework.

A consulting firm could build a playbook that encodes their methodology — intake, diagnosis, recommendation, delivery. A manufacturer could build one for quality audits. An agency could build one for client onboarding. Whatever your experts do repeatedly, a playbook can encode it.

See [PLAYBOOK-BLUEPRINT.md](PLAYBOOK-BLUEPRINT.md) for how to create skills that plug into the framework.

## Install

```bash
git clone https://github.com/aplayerlabs/playbooks.git ~/.claude/skills/playbooks
cd ~/.claude/skills/playbooks && ./setup
```

Then open Claude Code and type `/app`.

## How it works

Every skill operates on the **playfield** — your project folder. It starts with two files:

- **STATUS.md** — for the human. Plain English. Where you are, what's done, what's next.
- **SESH.md** — for the skills. Structured handoff data so each skill picks up where the last one left off.

Skills chain together through SESH.md. Each skill reads it, does its work, writes its section, and hands off to the next. The playfield grows as the project progresses — `/build` scaffolds the full structure (tasks, bugs, requirements, roadmap, docs) on first run.

`/wrap` saves progress at any point and generates a continuation plan. Pick up exactly where you left off, even across sessions.

## Philosophy

See [ETHOS.md](ETHOS.md) for why this exists.

## Architecture

See [ARCHITECTURE.md](ARCHITECTURE.md) for how skills chain together under the hood.

## Custom playbooks for your business

This repo is the open-source framework. If you want a playbook designed for your specific domain — your expertise, your workflow, your team — [A Player Labs](https://aplayerlabs.com) builds custom playbooks for businesses that want to scale what their experts know.

## License

MIT. See [LICENSE](LICENSE). Built by [A Player Labs](https://aplayerlabs.com).
