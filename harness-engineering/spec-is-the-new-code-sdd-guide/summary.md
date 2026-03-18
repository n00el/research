# The Spec Is the New Code: A Guide to Spec Driven Development

**Author:** Julian (@juliandeangeIis)
**Date:** 2026-03-15
**Source:** https://x.com/juliandeangeIis/status/2033303156340240481

## Key Takeaways

- AI coding agents fail because of ambiguous instructions and weak harnesses, not weak models -- every unspecified assumption becomes a silent decision the agent may get wrong
- SDD is a four-step methodology: Specify (functional what), Plan (technical how), Break down (ordered tasks), Implement (one task at a time) -- each step reduces ambiguity
- Three maturity levels: Spec-First (discard after delivery), Spec-Anchored (spec lives in repo and evolves), Spec-as-Source (edit spec, code regenerates)
- Separating functional specs from technical plans reduces LLM uncertainty; Given/When/Then acceptance criteria become the test plan agents can self-verify against
- Self-contained tasks unlock parallelism and agent agnosticism -- start with Claude Code, pick up a task with Cursor, finish with Codex
- MercadoLibre is rolling out SDD to 20,000 developers through hands-on workshops (5,000+ attendees so far), finding that practice beats explanation for driving adoption
- Tradeoff: SDD uses 2-3x more tokens upfront but produces dramatically better results for complex, multi-domain features

## One-Line Summary

Spec Driven Development is a four-step methodology (specify, plan, decompose, implement) that eliminates the ambiguity problem in AI coding by engineering the agent's entire context window before any code is written.

## Topics

spec driven development, harness engineering, agent coding, MercadoLibre, task decomposition, ambiguity reduction
