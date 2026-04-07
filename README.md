# Playbooks

Turn a business problem into a working application — without writing code.

Playbooks is a skill pack for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) that guides you from "I have a problem" to "my app is live" through a single playbook — eight specialist skills, in order, each one handing off to the next.

## Who this is for

Business owners who have a problem they want software to solve — or an idea they want to test. You're comfortable with a computer. You've never written code. You don't need to.

## How it's structured

**One playbook. Eight skills. Each skill contains plays.**

- The **playbook** is the whole thing — the end-to-end system that takes you from problem to live app.
- **Skills** are the eight specialist steps that chain together. Each skill is a slash command.
- **Plays** are the steps and moves within each skill — the modes, protocols, and decision trees.
- The **playfield** is the project folder structure that every skill reads from and writes to.

## The skill chain

```
/playbooks    Start here. Reads your project state and tells you what to do next.
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

At any point, type `/wrap` to pause and pick up later. Type `/status` to see where you are. After launch, re-enter the playbook at any skill to add features, fix bugs, or ship updates.

## Install

```bash
git clone https://github.com/aplayerlabs/playbooks.git ~/.claude/skills/playbooks
cd ~/.claude/skills/playbooks && ./setup
```

Then open Claude Code and type `/playbooks`.

## How it works

Every skill operates on the **playfield** — your project folder. It starts with two files:

- **STATUS.md** — for you. Plain English. Where you are, what's done, what's next.
- **SESH.md** — for the skills. Structured handoff data so each skill picks up where the last one left off.

When `/build` runs for the first time, it scaffolds the full playfield: tasks, bugs, requirements, roadmap, docs, and your application code. From that point on, every skill knows exactly where to find and file information.

You read STATUS.md. The skills read SESH.md. The playbook stays on track.

When a session ends, `/wrap` saves your progress and generates a continuation plan. Accept it and you're back to work — no copy-pasting, no lost context.

## YouTube

Each skill maps to an episode. Watch the series to see the full playbook in action.

## Build your own

Playbooks is extensible. See [PLAYBOOK-BLUEPRINT.md](PLAYBOOK-BLUEPRINT.md) for how to create custom skills that plug into the playbook.

## Philosophy

See [ETHOS.md](ETHOS.md) for why this exists and who it's for.

## Architecture

See [ARCHITECTURE.md](ARCHITECTURE.md) for how the skills work together.

## Custom playbooks for your business

Playbooks is the open-source proof of concept. If you want a playbook designed for your specific domain — your expertise, your workflow, your team — [A Player Labs](https://aplayerlabs.com) builds custom playbooks for businesses that want to scale their expertise.

## License

MIT. See [LICENSE](LICENSE). Built by [A Player Labs](https://aplayerlabs.com).
