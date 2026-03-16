# Instructions.md vs Skills.md vs Agent.md vs Agents.md

**Author:** Priyanka Vergadia (@pvergadia)
**Date:** 2026-03-16
**Source:** https://x.com/pvergadia/status/2033403840264126816

## Key Takeaways

- Multi-agent systems fail when agents lack clear definitions of identity, capabilities, boundaries, and workflows -- cramming everything into one system prompt causes drift and misbehavior
- **SKILL.md** (Training Manual): One file per capability, with explicit steps and failure modes -- keeps skills atomic and reusable across agents
- **Agent.md** (Employee Badge): Short per-agent identity file defining role, place in the system, capabilities, and critically, what the agent does NOT do -- the negative boundary is the most important part
- **AGENTS.md** (Org Chart): Single root-level file mapping the entire multi-agent system, giving every agent a mental model of the whole architecture so they optimize for the system, not just themselves
- **INSTRUCTIONS.md** (Employee Handbook): Operational behavioral rules and numbered workflows -- prose instructions get interpreted loosely, but numbered steps get followed precisely
- Agent misbehavior almost always comes from an agent filling in a gap that was not explicitly closed -- if you do not tell it what NOT to do, it will eventually do it
- Teams naturally write SKILL.md and INSTRUCTIONS.md but skip Agent.md and AGENTS.md, which are the files that prevent agents from defining their own roles unpredictably

## One-Line Summary

Multi-agent systems need four separate markdown files -- SKILL.md, Agent.md, AGENTS.md, and INSTRUCTIONS.md -- each answering a distinct question about capabilities, identity, system topology, and behavior, because agents that lack explicit boundaries will inevitably fill gaps on their own.

## Topics

agent-design, multi-agent-systems, prompt-engineering, markdown-configuration, agent-identity, agent-orchestration
