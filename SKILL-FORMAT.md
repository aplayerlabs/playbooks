# Skill File Format

Every playbook and utility in Playbooks is registered as a Claude Code slash command via a `SKILL.md` file with YAML frontmatter.

## Frontmatter spec

```yaml
---
name: playbook-name                    # The slash command name (becomes /playbook-name)
version: 1.0.0                      # Semantic version
description: |                      # One sentence shown in slash command discovery
  What this playbook does, in business-owner language.
position-in-pipeline: 3             # 0 = not in pipeline (utility), 1-8 = pipeline order
voice-triggers:                     # Speech-to-text aliases (optional)
  - "discover my problem"
  - "find my problem"
allowed-tools:                      # Claude Code tools this playbook may use
  - Bash
  - Read
  - Write
  - Edit
  - Glob
  - Grep
---
```

## Fields

| Field | Required | Type | Description |
|-------|----------|------|-------------|
| `name` | Yes | string | Slash command name. Lowercase, hyphens for spaces. |
| `version` | Yes | string | Semantic version (MAJOR.MINOR.PATCH). |
| `description` | Yes | string | One sentence. Business-owner language. No jargon. Shown when Claude Code lists available skills. |
| `position-in-pipeline` | Yes | integer | Pipeline order (1-8) or 0 for utilities. Used by /app to determine sequence. |
| `voice-triggers` | No | list | Alternate phrases that should trigger this playbook. For speech-to-text users. |
| `allowed-tools` | No | list | Claude Code tools the playbook is allowed to use. Omit for all tools. |

## Pipeline positions

| Position | Playbook |
|----------|-------|
| 0 | Utilities: /wrap, /status, /upgrade, /app |
| 1 | /discover |
| 2 | /plan |
| 3 | /setup |
| 4 | /define |
| 5 | /design |
| 6 | /build |
| 7 | /test |
| 8 | /launch |

## Body content

Below the frontmatter, the SKILL.md body tells Claude Code what to do when the skill is invoked. Typically this instructs Claude to read and follow the playbook's CLAUDE.md:

```markdown
---
name: discover
version: 1.0.0
description: |
  Validate your business problem or refine your idea into one.
position-in-pipeline: 1
---

Read and follow the operating contract at `playbooks/discover/CLAUDE.md`.

Start by checking for SESH.md in the current directory. If it exists, read it.
If not, create it — this is a new project.

Then follow the session start protocol in CLAUDE.md.
```

## Version bumping

- PATCH: bug fix in playbook behavior, clarification in docs
- MINOR: new mode added, playbook added, behavior enhancement
- MAJOR: breaking change to how the playbook operates, modes renamed or removed
