# Spec Decisions — /plan

## PD-001: Four tools embedded, not four separate commands

**Date:** 2026-04-06
**Decision:** Embed Premortem, Inversion, Bottleneck/Mitigation, and Optionality as internal processes within /plan rather than exposing them as standalone slash commands.
**Why:** The business owner audience doesn't need to know about or manage four separate thinking frameworks. They need one command that stress-tests their direction. Internally, /plan runs the tools in a deliberate sequence where each feeds the next — premortem failures inform inversion anti-patterns, both feed the bottleneck tree, and the optionality map uses everything above. Exposing them separately would force the business owner to understand the dependency chain and run them in order. /plan handles that automatically. The standalone skills remain available in the Playbooks skill library for power users or future v2 exposure.

## PD-002: Premortem is the mandatory gate

**Date:** 2026-04-06
**Decision:** /plan cannot mark itself DONE without at least one completed premortem. The other three tools can be skipped in FOCUSED mode.
**Why:** The premortem is the highest-leverage thinking tool. Inversion, bottleneck, and optionality all improve the analysis, but the premortem alone is sufficient to prevent the most common failure: building something without confronting the ways it could fail. A business owner in a hurry can run just the premortem and get 80% of the value. Making all four mandatory would create friction that causes business owners to skip /plan entirely — which is worse than running a partial stress test.

## PD-003: Business owner must acknowledge, not solve

**Date:** 2026-04-06
**Decision:** /plan requires the business owner to acknowledge the top risks but does not require them to have solutions or mitigation plans for all of them.
**Why:** Requiring solutions would turn /plan into a blocking gate that most business owners would skip or game. The goal is informed risk-taking, not risk elimination. A business owner who says "I see the risk and I'm going anyway" is making a better decision than one who never saw the risk. The bottleneck tree offers pre-decided responses where possible, but "Decide when: [condition]" is a valid entry — some risks need future information to resolve. Acknowledgment is the minimum bar: you saw it, you considered it, you're choosing your path with eyes open.
