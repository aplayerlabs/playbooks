# Usage — /design

Show the business owner what their app will look like before code is written.

---

## Location

`playbooks/design/` inside the Playbooks skill pack.

Do not copy into projects. The playbook reads from and writes to the project directory.

---

## Starting a Session

### MOCKUP mode (default)

```
/design
```

Generates HTML preview pages from the PRD. Business owner opens them in a browser.

### DESIGN mode (Figma)

```
/design DESIGN
```

Generates DTCG tokens and Figma Scripter code. Requires Figma Desktop + Scripter plugin.

### REFINE mode

```
/design REFINE
```

Iterate on existing mockups or designs based on business owner feedback.

---

## What Happens

### HTML path (most business owners)

1. /design reads the PRD and identifies 2-4 key screens
2. Generates standalone HTML files in `mockups/` plus a shared `_styles.css`
3. Tells you: "Open `mockups/01-dashboard.html` in your browser"
4. You look at it and say what to change
5. /design updates the mockups
6. When you're happy, the design is "approved" and /build can start

### Figma path (if you use Figma)

1. /design reads the PRD and identifies key screens
2. Generates a Scripter code block with tokens, layers, and bindings
3. You paste it into Figma's Scripter plugin and click Play
4. Review the result in Figma, give feedback
5. /design generates fix scripts if needed
6. When approved, /build starts

---

## What Goes Where

| Location | Contains | Who creates it |
|----------|----------|----------------|
| Project root `mockups/` | HTML preview pages, shared CSS | /design |
| Project root `SESH.md` `## Design` | Design system, screen list, approval status | /design |
| Project root `STATUS.md` | Plain English design status | /design |
| `prd/prd.md` | Requirements (read-only input) | /define |

---

## External Steps

### HTML mockups

The only external step: **open the HTML file in your browser.** On Mac, double-click the file or drag it into Chrome/Safari/Firefox. On Windows, same thing.

### Figma (if you choose that path)

1. Go to figma.com and create a free account
2. Download Figma Desktop (the browser version works but Desktop is smoother)
3. Create a new file
4. Install Scripter: Plugins menu > Search "Scripter" > Install
5. Open Scripter: Plugins > Scripter
6. Paste the code /design gives you into the editor
7. Click the Play button (triangle icon)
8. Check the notification at the bottom for results

---

## Pipeline Context

| Playbook | Relationship to /design |
|-------|------------------------|
| **/define** (before) | Produces the PRD that /design reads. If the PRD is missing or vague, /design will suggest running /define first. |
| **/build** (after) | Implements the design system documented in SESH.md. /build reads the colors, fonts, and spacing from `## Design` — not from the mockup HTML files. |
| **/wrap** | Saves design progress mid-session. Generates a continuation prompt to resume with `/design REFINE`. |

---

## Direct Entry

You can run /design without going through the full pipeline. If there's no PRD, /design can work from a description — but the result will be less precise. If there's no SESH.md, /design creates one.

---

## Tips

- **Start with MOCKUP mode.** It's faster and doesn't require any tools beyond a web browser.
- **React to mockups with specifics.** "The sidebar is too wide" is better than "I don't like the layout."
- **Don't worry about perfection.** Mockups confirm direction. /build handles the polish.
- **Three screens is usually enough.** If you need more, /design will add them. But start with the core.
