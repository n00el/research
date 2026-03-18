# Instructions.md vs Skills.md vs Agent.md vs Agents.md

**Author:** Priyanka Vergadia (@pvergadia)
**Date:** 2026-03-16
**Source:** https://x.com/pvergadia/status/2033403840264126816

## Key Takeaways

- Multi-agent systems fail in production because agents fill gaps that weren't explicitly closed -- the fix is four markdown files, each answering one question
- SKILL.md (training manual): one per capability, includes step-by-step processes AND failure modes -- best skills describe what to do when things go wrong
- Agent.md (employee badge): per-agent identity grounding; the "What I do NOT do" section is the most important part, preventing role drift
- AGENTS.md (org chart): single root-level file giving every agent a mental model of the entire system, enabling natural output formatting for downstream consumers
- INSTRUCTIONS.md (employee handbook): numbered workflow steps get followed precisely while prose instructions get interpreted loosely; separate "always/never" rules from "first/then/finally" workflows
- Teams consistently skip Agent.md and AGENTS.md because they feel administrative, but without identity grounding, agents define their own roles and drift into chaos

## One-Line Summary

Four markdown files -- SKILL.md (capabilities), Agent.md (identity), AGENTS.md (system map), and INSTRUCTIONS.md (behavioral rules) -- solve multi-agent production chaos by giving each agent explicit answers to what it can do, who it is, where it fits, and how it should behave.

## Topics

multi-agent systems, agent identity, SKILL.md, agent architecture, production debugging, separation of concerns
