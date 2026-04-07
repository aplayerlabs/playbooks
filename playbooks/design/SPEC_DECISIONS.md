# Spec Decisions — /design

Why the rules exist. Prefix: DD (Design Decisions).

---

## DD-001: HTML Mockups as Default Path

**Date:** 2026-04-06

**Decision:** Default to standalone HTML/CSS preview pages, not Figma.

**Why:** The target audience is business owners who can't code. Most of them don't have Figma, have never used Figma, and shouldn't need to learn a design tool to see what their app will look like. Every computer has a web browser. Double-clicking an HTML file is the lowest-friction path from "I wonder what it'll look like" to "I can see it." Figma is offered as an optional upgrade for those who already use it.

---

## DD-002: Mockups Are Disposable

**Date:** 2026-04-06

**Decision:** HTML mockups in `mockups/` are throwaway artifacts. /build does not copy or reference them.

**Why:** If /build reads mockup HTML, the mockups become a second source of truth competing with the PRD and design system. That creates drift — someone changes a mockup but not the design system, or vice versa. Instead, /build reads the design system from SESH.md `## Design` (colors, fonts, spacing as named values) and implements from that. The mockups exist only for the business owner to look at during the design phase. After approval, they're dead weight. Clean separation.

---

## DD-003: Design System in SESH.md, Not a Separate File

**Date:** 2026-04-06

**Decision:** Document the design system (colors, typography, spacing) directly in SESH.md `## Design`, not in a standalone `design-system.md` or `tokens.json`.

**Why:** SESH.md is the skill-to-skill contract. /build reads SESH.md. If the design system lives in a separate file, /build needs to know to look for it, and there's no guarantee the file name or location is consistent across projects. Putting it inline in the section /build already reads guarantees it's found. For the Figma path, tokens.json exists as a byproduct of Scripter code generation, but the canonical reference is still SESH.md.

---

## DD-004: Three Modes, Not Two

**Date:** 2026-04-06

**Decision:** Separate MOCKUP, DESIGN, and REFINE into distinct modes rather than collapsing REFINE into the other two.

**Why:** MOCKUP and DESIGN have different toolchains (HTML files vs. Figma Scripter). REFINE works on either path and has different behavior — it reads existing work and iterates rather than generating from scratch. Mixing "create" and "iterate" in the same mode risks the playbook regenerating everything when the business owner just wants to tweak a header color. Explicit modes make the intent clear.

---

## DD-005: Ask About Figma Once, Then Remember

**Date:** 2026-04-06

**Decision:** Ask the business owner whether they use Figma at session start. Record the answer in SESH.md `## Design` under Path. Don't ask again.

**Why:** Asking every session is annoying. Asking mid-flow breaks momentum. The answer doesn't change between sessions. Record it once and use it for all future /design sessions on this project.

---

## DD-006: Binary Feedback Questions

**Date:** 2026-04-06

**Decision:** Present mockups with yes/no questions ("Does this layout feel right?") rather than open-ended ("What do you think?").

**Why:** Non-designers freeze on open-ended design questions. They don't have the vocabulary to articulate what they want — but they know what's wrong when they see it. Binary questions give them a handle: "Yes" means move on. "No" triggers a follow-up: "What feels off?" This gets to actionable feedback faster than waiting for unprompted critique.

---

## DD-007: Mockup Banner

**Date:** 2026-04-06

**Decision:** Every HTML mockup includes a visible "MOCKUP — NOT THE FINAL APP" banner at the top.

**Why:** Without the banner, business owners sometimes confuse the mockup with the real app. They share the HTML file with their team and say "here's the app." Then when /build produces something that looks slightly different (because it's real code, not a static page), there's confusion. The banner sets expectations explicitly every time they look at it.

---

## DD-008: CSS Custom Properties Mirror the Design System

**Date:** 2026-04-06

**Decision:** HTML mockups use CSS custom properties (`--color-primary`, `--spacing-md`, etc.) that map 1:1 to the design system documented in SESH.md.

**Why:** This creates a direct translation path. When /build reads the design system from SESH.md, the naming convention is already established. `/build` can implement `--color-primary` as a Tailwind theme value or CSS variable using the same name. It also makes mockup iteration faster — changing one value in `_styles.css` updates every page that uses it.

---

## Template

```markdown
## DD-XXX: {Title}

**Date:** YYYY-MM-DD

**Decision:** What you decided.

**Why:** Why it made sense.
```
