# Lessons from Building Claude Code: Prompt Caching Is Everything

**Author:** Thariq (@trq212)
**Date:** February 19, 2026
**Source:** https://x.com/trq212/status/2024574133011673516

## Key Takeaways

- Order content as static-first, dynamic-last (system prompts, then project context, then session data, then conversation) to maximize prefix-based cache hits across turns.
- Never modify the system prompt mid-conversation; instead inject updated information as system-reminder messages in later turns to preserve the cached prefix.
- Avoid switching models mid-conversation as it invalidates the entire cache; use subagents for model transitions to keep the parent cache intact.
- Keep tool sets stable throughout a conversation -- use state-transition tools and defer_loading stubs instead of adding or removing tools, which breaks caching.
- Prompt caching reduces long Opus session costs from $50-100 down to $10-19, making agentic products economically viable at subscription price points.
- Treat cache hit rate as a critical infrastructure metric with alerting and SEV declarations on drops, just like uptime monitoring.
- When compacting a full context window, reuse the parent conversation's system prompt and tools in the new session to maintain cache prefix matches.

## One-Line Summary

Agentic LLM products must be architected around prompt caching from the ground up, ordering content by stability and never mutating cached prefixes, because cache hit rates directly determine whether the product is economically viable.

## Topics

prompt caching, agent architecture, claude code, cost optimization, llm infrastructure, context management
