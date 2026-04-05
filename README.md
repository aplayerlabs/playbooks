# A Player Brains

Turn a business problem into a working application — without writing code.

A Player Brains is a skill pack for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) that guides you from "I have a problem" to "my app is live" through a brain chain — a coordinated sequence of specialist AI brains where each brain's output feeds the next. Each brain handles one stage. You follow the track.

## Who this is for

Business owners who have identified a bottleneck and want software to solve it. You're comfortable with a computer. You've never written code. You don't need to.

A Player Brains makes the decisions that normally require a developer — what framework, what database, what hosting, how to deploy. You make the product decisions. The brains handle the rest.

## The brain chain

```
/aplayerbrains    Start here. Reads your project state and tells you what to do next.
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

At any point, type `/wrap` to pause and pick up later. Type `/status` to see where you are.

## Install

```bash
git clone https://github.com/aplayerlabs/aplayerbrains.git ~/.claude/skills/aplayerbrains
cd ~/.claude/skills/aplayerbrains && ./setup
```

Then open Claude Code and type `/aplayerbrains`.

## How it works

Every brain reads and writes two files in your project:

- **STATUS.md** — for you. Plain English. Where you are, what's done, what's next.
- **SESH.md** — for the brains. Structured handoff data so each brain picks up where the last one left off.

You read STATUS.md. The brains read SESH.md. The brain chain stays on track.

## YouTube

Each brain maps to an episode. Watch the series to see the full brain chain in action.

[Episode list coming soon]

## Build your own brain

A Player Brains is extensible. See [BRAIN-BLUEPRINT.md](BRAIN-BLUEPRINT.md) for how to create custom brains that plug into the brain chain.

## Philosophy

See [ETHOS.md](ETHOS.md) for why this exists and who it's for.

## Architecture

See [ARCHITECTURE.md](ARCHITECTURE.md) for how the brains work together.

## Custom brain chains for your business

A Player Brains is the open-source proof of concept. If you want a brain chain designed for your specific domain — your expertise, your workflow, your team — [A Player Labs](https://aplayerlabs.com) builds custom brain chains for businesses that want to scale their expertise.

## License

MIT. See [LICENSE](LICENSE).

## A Player Labs

Built by [A Player Labs](https://aplayerlabs.com). We build AI brains that give business owners leverage.
