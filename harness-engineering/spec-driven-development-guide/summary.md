# The Spec Is the New Code: A Guide to Spec Driven Development

**Author:** Julian (@juliandeangeIis)
**Date:** 2026-03-15
**Source:** https://x.com/juliandeangeIis/status/2033303156340240481

## Key Takeaways

- AI coding agents fail not because models are weak, but because instructions are ambiguous and the agent harness is too weak -- the spec is the real bottleneck, not the model.
- SDD follows a four-phase flow (Specify -> Plan -> Tasks -> Implement) where ambiguity decreases at each step, giving the agent progressively clearer instructions.
- The spectrum of SDD has three levels: Spec-First (temporary, discarded after delivery), Spec-Anchored (lives in the repo, evolves with code), and Spec-as-Source (spec is the primary artifact, code is derived from it).
- A Technical Implementation Guide produced via codebase scan should include architectural decisions, data models, auth patterns, MCP integrations, and performance constraints before the agent writes any code.
- SDD costs more upfront (2-3x tokens, planning time, spec-plan-task cycle) but delivers dramatic performance boosts, reduced ambiguity, and accurate complex features.
- SDD is not for everything -- small changes, quick bug fixes, and config updates should use Plan Mode or direct prompts. SDD shines on complex features, multi-file changes, and multi-domain logic.
- Vague prompts cause agents to guess at architecture, data models, and auth patterns, leading to rework; detailed specs with concrete constraints (idempotency, API schema, performance limits) produce correct implementations on the first try.

## One-Line Summary

Spec Driven Development treats the specification as the primary engineering artifact, systematically reducing ambiguity across three maturity levels so coding agents build the right thing the first time instead of guessing from vague prompts.

## Topics

Spec-driven development, coding agents, agent harness, ambiguity reduction, technical specifications, AI-assisted development, implementation planning
