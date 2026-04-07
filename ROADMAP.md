# Roadmap

## Shipping Phases

### Phase A — Foundation + Thinking Playbooks
The front door, the thinking pipeline, and utilities. Useful immediately.

- [x] Repo structure and skeleton
- [x] Foundation docs (README, ETHOS, ARCHITECTURE, PLAYBOOK-BLUEPRINT, SECURITY, GETTING-STARTED)
- [x] Spec docs (SKILL-FORMAT, PROGRESS-SIGNALING)
- [x] Playfield templates (SESH.md, STATUS.md)
- [x] Playbook stubs with frontmatter (all 11)
- [ ] Setup script (functional install + slash command registration)
- [ ] /app CLAUDE.md (front door router)
- [ ] /discover CLAUDE.md + USAGE.md + SPEC_CHANGELOG.md + SPEC_DECISIONS.md
- [ ] /plan CLAUDE.md + USAGE.md + SPEC_CHANGELOG.md + SPEC_DECISIONS.md
- [ ] /wrap CLAUDE.md + USAGE.md + SPEC_CHANGELOG.md + SPEC_DECISIONS.md
- [ ] /status CLAUDE.md + USAGE.md + SPEC_CHANGELOG.md + SPEC_DECISIONS.md
- [ ] /upgrade CLAUDE.md + USAGE.md + SPEC_CHANGELOG.md + SPEC_DECISIONS.md
- [ ] Quality gate: fresh install test
- [ ] Quality gate: discover → plan pipeline test
- [ ] Quality gate: wrap/re-entry validation
- [ ] Quality gate: /status at every stage
- [ ] Quality gate: direct entry (every Phase A skill)
- [ ] YouTube Ep 0: Getting Started (script)
- [ ] YouTube Ep 1: Finding the Real Problem (script)
- [ ] YouTube Ep 2: Stress-Testing Before You Build (script)
- [ ] Negative testing and hardening pass

### Phase B — Setup + Define + Design
Infrastructure, requirements, and visual validation. Prepares everything before code.

- [ ] /setup CLAUDE.md + USAGE.md + SPEC_CHANGELOG.md + SPEC_DECISIONS.md
- [ ] /setup external steps documentation (GitHub account, Render account, API keys)
- [ ] /define CLAUDE.md + USAGE.md + SPEC_CHANGELOG.md + SPEC_DECISIONS.md
- [ ] /design CLAUDE.md + USAGE.md + SPEC_CHANGELOG.md + SPEC_DECISIONS.md
- [ ] /design non-Figma path (HTML/CSS preview mockups)
- [ ] /design external steps documentation (Figma account, Scripter plugin)
- [ ] Config system (~/.apb/config.yaml)
- [ ] Full project document structure (tasks/, bugs/, requirements/, roadmap/, docs/)
- [ ] STATUS.md extension (build/test/launch fields)
- [ ] Quality gate: setup → define → design pipeline test
- [ ] Quality gate: platform validation (Render + Vercel)
- [ ] Quality gate: direct entry (every Phase B skill)
- [ ] YouTube Ep 3: Setting Up Your Infrastructure (script)
- [ ] YouTube Ep 4: Writing the Blueprint (script)
- [ ] YouTube Ep 5: Designing What You'll Build (script)
- [ ] Negative testing and hardening pass

### Phase C — Build + Test + Launch
The building, breaking, and shipping playbooks. Completes the pipeline.

- [ ] /build CLAUDE.md + USAGE.md + SPEC_CHANGELOG.md + SPEC_DECISIONS.md
- [ ] /build BOOTSTRAP mode (PRD → scaffolded project)
- [ ] /build BUILD mode (autonomous task execution)
- [ ] /build FIX mode (targeted bug fixes from /test)
- [ ] /test CLAUDE.md + USAGE.md + SPEC_CHANGELOG.md + SPEC_DECISIONS.md
- [ ] /test playbooks (edge cases, security, performance, accessibility)
- [ ] /test executive summary and ship readiness (RED/YELLOW/GREEN)
- [ ] /launch CLAUDE.md + USAGE.md + SPEC_CHANGELOG.md + SPEC_DECISIONS.md
- [ ] /launch Render integration
- [ ] /launch Vercel integration
- [ ] SECURITY.md refinement based on /build patterns
- [ ] Full example walkthrough (problem → live app)
- [ ] Quality gate: end-to-end pipeline test (all 8 skills)
- [ ] Quality gate: non-technical user full pipeline test
- [ ] Quality gate: multi-session /build (5+ sessions)
- [ ] Quality gate: test → build → test loop
- [ ] Quality gate: SESH.md accumulation (all 8 sections populated)
- [ ] Quality gate: /upgrade delivers Phase C to Phase A/B users
- [ ] YouTube Ep 6: Building Your App Part 1 (script)
- [ ] YouTube Ep 7: Building Your App Part 2 (script)
- [ ] YouTube Ep 8: Breaking Your App (script)
- [ ] YouTube Ep 9: Going Live (script)
- [ ] YouTube Ep 10: Build Your Own Playbook (script)
- [ ] Negative testing and hardening pass

## Future (v2+)

- Animator skill (motion polish)
- Capturer skill (design state capture)
- Standalone thinking skills (/premortem, /inversion, /bottleneck, /optionality, /delegate, /document)
- Multi-host support (Codex, Cursor)
- Community playbook marketplace
