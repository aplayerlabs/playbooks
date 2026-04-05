# Contributing

How to contribute to A Player Brains.

## How to contribute

### What's welcome

- New brains that extend the pipeline or serve specific domains
- Playbook improvements (testing patterns, design patterns, etc.)
- Bug fixes in existing brain specs
- Documentation improvements
- YouTube episode suggestions or scripts

### What's not welcome (without discussion first)

- Changes to the pipeline order
- Changes to the SESH.md or STATUS.md contract format
- Removing sections from existing brains
- Changing the 14-section CLAUDE.md structure

If you want to propose changes in any of these areas, open an issue first and explain the reasoning. We'll discuss before any code is written.

### How to submit

1. Fork the repo
2. Create a branch from `staging` (not main)
3. Make your changes
4. Verify against the [brain blueprint checklist](BRAIN-BLUEPRINT.md#checklist)
5. Submit a PR to `staging`
6. Fill out the PR template

### Branch structure

- `main` — stable release. What users install. Don't target PRs here.
- `staging` — integration branch. PRs land here. Tested before promoting to main.
- `dev` — maintainer's active development. Don't target PRs here.

## Creating a new brain

1. Read [BRAIN-BLUEPRINT.md](BRAIN-BLUEPRINT.md)
2. Create a folder in `brains/your-brain-name/`
3. Create all required files: `SKILL.md`, `CLAUDE.md`, `USAGE.md`, `SPEC_CHANGELOG.md`, `SPEC_DECISIONS.md`
4. Follow the 14-section structure for CLAUDE.md
5. Use a unique 2-letter prefix for spec decisions (check existing brains to avoid collisions)
6. Test your brain by invoking it in Claude Code
7. Submit a PR to `staging` with the brain blueprint checklist completed

## Code of conduct

Be constructive. The goal is helping business owners build software. Keep that audience in mind when contributing.

## Questions?

Open an issue on GitHub or reach out to the team at [aplayerlabs.com](https://aplayerlabs.com).
