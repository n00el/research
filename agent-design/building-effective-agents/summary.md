# Building Effective Agents

**Author:** Erik Schluntz and Barry Zhang
**Date:** December 19, 2024
**Source:** https://www.anthropic.com/engineering/building-effective-agents

## Key Takeaways

- The most successful agent implementations use simple, composable patterns rather than complex frameworks -- start with direct LLM API calls before reaching for abstractions.
- Anthropic distinguishes between **workflows** (predefined code paths orchestrating LLMs) and **agents** (LLMs dynamically directing their own processes), and recommends using the simplest approach that works.
- Five core workflow patterns cover most use cases: prompt chaining, routing, parallelization, orchestrator-workers, and evaluator-optimizer -- each with clear "when to use" criteria.
- Tool design (Agent-Computer Interface / ACI) deserves as much engineering attention as prompt design -- the SWE-bench team spent more time optimizing tools than the overall prompt.
- Autonomous agents should only be used for open-ended problems where steps cannot be predicted; they bring higher costs and compounding error risks that require sandboxed testing and guardrails.
- Customer support and coding are two domains where agents have shown the most practical production value, because both have clear success criteria and built-in feedback loops.
- The "poka-yoke" principle applies to tool design: make it structurally harder for the model to make mistakes (e.g., requiring absolute file paths instead of relative ones).

## One-Line Summary

Effective LLM agents are built from simple, composable workflow patterns with carefully engineered tool interfaces, not complex frameworks -- and complexity should only be added when it demonstrably improves outcomes.

## Topics

AI agents, LLM architecture, prompt engineering, tool design, agentic workflows, software engineering
