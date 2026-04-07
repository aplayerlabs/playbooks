# Usage — /build

---

## Location

This skill lives in the Playbooks skill pack:

```
~/.claude/skills/playbooks/playbooks/build/
```

Do not copy it into project folders. It stays here and gets invoked via slash command.

---

## Starting a Session

### First time on a new project (BOOTSTRAP)

```
/build
```

If no project structure exists, /build automatically enters BOOTSTRAP mode. It reads the PRD, picks the stack, scaffolds the playfield, creates Stage 0 tasks, and sets you up to build.

You can also be explicit:

```
/build BOOTSTRAP
```

### Continuing work (BUILD)

```
/build
```

BUILD is the default mode. If the project is already bootstrapped and tasks exist, /build picks up where it left off. Read SESH.md, grab the next task, start coding.

After a /wrap:

```
/build
```

The continuation prompt from /wrap tells /build exactly where to resume. Just invoke it and it reads the state.

### Fixing bugs from /test (FIX)

```
/build FIX
```

Reads bugs/open.md, prioritizes by severity, fixes them. Targeted mode — no new features, just fixes.

---

## What Goes Where

| Location | Contains | In the project repo? |
|----------|----------|---------------------|
| `~/.claude/skills/playbooks/playbooks/build/` | The playbook's operating contract | Never — this is the machine |
| `~/your-project/` | App code, tasks, bugs, docs, SESH.md, STATUS.md | Yes — this is the deliverable |

---

## What /build Creates (BOOTSTRAP) — The Playfield

On the first session, /build scaffolds the full **playfield** — the project folder structure that every skill operates through:

```
your-project/                   ← the playfield
  src/                    ← application code
  tasks/                  ← backlog, active, done
  bugs/                   ← open, investigating, resolved
  requirements/           ← stage-0.md (from PRD)
  roadmap/                ← roadmap.md with stages
  docs/                   ← architecture, decisions, environments, risks
  CHANGELOG.md            ← version history
  .env.example            ← required env vars (no values)
  .gitignore              ← sensible defaults
  Dockerfile              ← explicit build config
```

SESH.md and STATUS.md already exist from earlier skills (or /build creates them if entering directly).

---

## How It Works With Other Playbooks

### Normal pipeline flow

```
/design finishes → business owner approved visual direction
/build BOOTSTRAP → scaffolds playfield, creates tasks
/build BUILD → executes tasks across multiple sessions
  /wrap → saves progress, generates continuation prompt
  /build → resumes from continuation prompt
  (repeat as many sessions as needed)
/build finishes Stage 0 → STATUS.md says "ready for /test"
/test → breaks the app, files bugs
  /wrap → continuation to /build FIX
/build FIX → fixes bugs from /test
  /wrap → continuation to /test VERIFY
/test VERIFY → confirms fixes
/launch → ships it
```

### Direct entry (no prior pipeline)

/build works standalone if you point it at a project with a PRD:

```
/build
```

If there's no SESH.md, /build creates one and backfills from whatever exists. If there's no PRD, it asks what you want to build and works from a description — less structured, but functional.

### The /wrap loop

/build sessions can be long. When context fills up or you need to stop:

```
/wrap
```

This saves progress to SESH.md and STATUS.md, and generates a continuation prompt. Next session, invoke /build and it picks up exactly where it left off.

---

## Reading Progress Between Sessions

You don't need to read code to know where things stand.

**STATUS.md** — plain English. Open it anytime. It tells you what's done, what's next, and any blockers.

**SESH.md** — more technical. Shows the architecture, task list, current work, files changed. Useful if you want detail.

---

## Modes at a Glance

| Mode | When | What it does |
|------|------|-------------|
| BOOTSTRAP | First session | Picks stack, scaffolds playfield, creates tasks |
| BUILD | Every other session | Executes tasks, writes code, tracks progress |
| FIX | After /test finds bugs | Targeted bug fixes, no new features |
