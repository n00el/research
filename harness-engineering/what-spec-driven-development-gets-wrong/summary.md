# What spec-driven development gets wrong

**Author:** Augment Code (@augmentcode)
**Date:** 2026-02-23
**Source:** https://x.com/augmentcode/status/2025993446633492725

## Key Takeaways

- Documentation always goes stale because keeping written artifacts in sync with changing code is invisible work that engineers deprioritize -- process, tooling, and team values have all failed to fix this.
- Spec-driven development (SDD) with coding agents amplifies the stale-docs problem: a stale spec misleads agents that will confidently execute an outdated plan without flagging anything wrong.
- The fix is making specs bidirectional: humans describe intent and approve plans, agents draft specs and write back findings, deviations, and decisions as they work.
- The right communication granularity matters -- agents should surface direction-changing decisions (like discovering an existing auth context to reuse) rather than narrating every line of code.
- Augment Code's "Intent" product implements this pattern: a coordinator agent breaks work into tasks, agents execute and update the spec, developers can pause/edit/approve at any point.
- The spec becomes a living document that reflects what was actually built, not what was originally planned, because both sides maintain it.

## One-Line Summary

Spec-driven development will fail just like every other documentation-first initiative unless agents actively write back to the spec as they discover reality diverges from the plan.

## Topics

AI agents, spec-driven development, developer workflow, documentation, coding agents, Augment Code
