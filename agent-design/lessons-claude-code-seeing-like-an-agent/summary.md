# Lessons from Building Claude Code: Seeing like an Agent

**Author:** Thariq Shihipar (@trq212)
**Date:** February 27, 2026
**Source:** https://x.com/trq212/status/2027463795355095314

## Key Takeaways

- Design tools to match the model's actual abilities, not theoretical use cases — the best tool is useless if the model doesn't understand how to call it reliably.
- Keep the tool count as low as possible; Claude Code has ~20 tools and the bar to add a new one is high because each additional tool increases the decision space the model must reason over.
- As model capabilities improve, previously essential tools (like TodoWrite with periodic reminders) can become constraints — revisit tool design assumptions with every model upgrade.
- Prefer progressive disclosure over bloating the system prompt: let agents discover context incrementally through skills, docs links, or specialized subagents rather than front-loading everything.
- Replacing RAG with agent-driven search (Grep) was a key unlock — giving models the ability to build their own context scales better than pre-indexing and handing them results.
- When a tool design fails (structured output in markdown, overloaded parameters), iterate on the interaction pattern rather than forcing the model to comply — the AskUserQuestion journey took three attempts before landing on a dedicated blocking tool.

## One-Line Summary

Agent tool design is an empirical discipline where you must continuously reshape the action space to match evolving model capabilities, favoring fewer well-matched tools and progressive context discovery over exhaustive upfront instrumentation.

## Topics

agent design, tool design, claude code, progressive disclosure, llm engineering, developer tools
