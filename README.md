# A Player Playbooks

Turn a business problem into a working application — without writing code.

A Player Playbooks is a skill pack for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) that guides you from "I have a problem" to "my app is live" through a playbook chain — eight specialist AI playbooks, in order, each one handing off to the next.

## Who this is for

Business owners who have a problem they want software to solve — or an idea they want to test. You're comfortable with a computer. You've never written code. You don't need to.

A Player Playbooks makes the decisions that normally require a developer — what framework, what database, what hosting, how to deploy. You make the product decisions. The playbooks handle the rest.

## The playbook chain

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

At any point, type `/wrap` to pause and pick up later. Type `/status` to see where you are. After launch, re-enter the chain at any playbook to add features, fix bugs, or ship updates.

## Install

```bash
git clone https://github.com/aplayerlabs/playbooks.git ~/.claude/skills/playbooks
cd ~/.claude/skills/playbooks && ./setup
```

Then open Claude Code and type `/playbooks`.

## How it works

The playbook operates through a **playfield** — a project folder structure that every skill reads from and writes to. It starts with two files:

- **STATUS.md** — for you. Plain English. Where you are, what's done, what's next.
- **SESH.md** — for the skills. Structured handoff data so each skill picks up where the last one left off.

When `/build` runs for the first time, it scaffolds the full playfield: tasks, bugs, requirements, roadmap, docs, and your application code. From that point on, every skill knows exactly where to find and file information.

You read STATUS.md. The skills read SESH.md. The playbook chain stays on track.

When a session ends, `/wrap` saves your progress and presents a plan to continue. Accept it and you're back to work with a fresh start — no copy-pasting, no lost context.

## YouTube

Each playbook maps to an episode. Watch the series to see the full playbook chain in action.

## Build your own playbook

A Player Playbooks is extensible. See [PLAYBOOK-BLUEPRINT.md](PLAYBOOK-BLUEPRINT.md) for how to create custom playbooks that plug into the playbook chain.

## Philosophy

See [ETHOS.md](ETHOS.md) for why this exists and who it's for.

## Architecture

See [ARCHITECTURE.md](ARCHITECTURE.md) for how the playbooks work together.

## Custom playbook chains for your business

A Player Playbooks is the open-source proof of concept. If you want a playbook chain designed for your specific domain — your expertise, your workflow, your team — [A Player Labs](https://aplayerlabs.com) builds custom playbook chains for businesses that want to scale their expertise.

## License

MIT. See [LICENSE](LICENSE). Built by [A Player Labs](https://aplayerlabs.com).
