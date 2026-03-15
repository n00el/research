# Lessons from Building Claude Code: Prompt Caching Is Everything

**Author:** Thariq (@trq212)
**Date:** February 19, 2026
**Source:** https://x.com/trq212/status/2024574133011673516
**Stats:** 151 replies, 426 retweets, 4,681 likes

---

## Overview

Long-running agentic products like Claude Code are made feasible by prompt caching, which allows reusing computation from previous roundtrips and significantly decreases latency and cost. At Claude Code, the entire harness is built around prompt caching. A high prompt cache hit rate decreases costs and helps create more generous rate limits for subscription plans. The team runs alerts on prompt cache hit rate and declares SEVs if they drop too low.

Without prompt caching, a long Opus coding session can cost $50-100 in input tokens. With prompt caching, the cost drops to $10-19.

## Core Principle: Cache Rules Everything Around Me

Prompt caching works by prefix matching. The optimal approach is: **static content first, dynamic content last**. This maximizes the reusable cached prefix across conversation turns.

## Architecture: Content Ordering Strategy

The optimal content ordering for maximum cache hits:

1. **Static system prompts and tools** (globally cached, shared across all sessions)
2. **Project-level context** (CLAUDE.md, project rules)
3. **Session-level data**
4. **Conversation messages** (most dynamic, placed last)

This layered approach means changes to later layers don't invalidate the cache for earlier layers.

## Lesson: System Message Updates

Rather than modifying the system prompt when information becomes outdated (which would cause a cache miss), insert system messages in subsequent conversation turns. This preserves the cached prefix while still communicating updated information like timestamps.

Use a `system-reminder` tag pattern to inject updated context without breaking the cached prefix.

## Lesson: Model Consistency

Switching models mid-conversation invalidates the entire cache. Instead, use **subagents** for model transitions -- one model prepares a handoff message to another model, keeping the parent conversation's cache intact.

## Lesson: Stable Tool Sets

Adding or removing tools mid-conversation breaks caching, since tools are part of the cached prefix. Instead of removing tools:

- Keep all tools present throughout the conversation
- Use tool-based state transitions (like `EnterPlanMode`) rather than swapping tool sets
- Implement `defer_loading` with lightweight stubs -- send minimal tool definitions that expand only when called

## Lesson: Cache-Safe Forking for Compaction

When the context window fills up, a **compaction** operation summarizes the conversation to continue in a fresh session. The cache-safe approach reuses the parent conversation's system prompt and tools in the compacted session, maintaining prefix matches during the summarization process.

## Lesson: Monitoring as a Critical Metric

Organizations should treat cache hit rates as critical infrastructure metrics, similar to uptime monitoring. Implement alerts for significant cache rate drops and treat them with the same urgency as service outages.

## Follow-Up Post

Thariq followed up on February 20, 2026 ([@trq212](https://x.com/trq212/status/2024638793719177291)):

> "one of the biggest realizations I've had working on Claude Code is that you fundamentally have to design agents for prompt caching first, almost every feature touches on it somehow. I wrote this in a day but it's the culmination of months of learnings, hope you enjoy it"

(90 replies, 232 retweets, 4,234 likes)
