# I want to become a Claude architect (full course).
**Author:** hoeem (@hooeem)
**Date:** March 15, 2026
**Source:** https://x.com/hooeem/status/2033198345045336559

## Key Takeaways
- The Claude Certified Architect (Foundations) exam covers 5 domains: Agentic Architecture (27%), Claude Code Config (20%), Prompt Engineering (20%), Tool Design & MCP (18%), and Context Management (15%) -- but the exam itself is restricted to Claude partners only.
- Subagents do NOT share memory with the coordinator; every piece of information must be passed explicitly in the prompt -- this is the single most commonly misunderstood concept in multi-agent systems.
- For high-stakes operations (financial, security, compliance), prompt-based guidance is insufficient -- you must use programmatic enforcement via hooks and prerequisite gates.
- Tool descriptions are the primary mechanism Claude uses for tool selection; vague or overlapping descriptions cause misrouting, and the fix is better descriptions (not routing classifiers or few-shot examples).
- The CLAUDE.md hierarchy (user/project/directory levels) plus .claude/rules/ with glob patterns is critical for team-wide Claude Code configuration; user-level config being unshared is the most common team setup failure.
- Few-shot examples (2-4 targeted examples with reasoning) are the highest-leverage technique for consistent structured output and reducing hallucination in extraction tasks.
- The article includes full study prompts for each of the 5 exam domains that can be pasted directly into Claude for self-guided learning.

## One-Line Summary
A comprehensive breakdown of the Claude Certified Architect exam curriculum covering agentic architecture, MCP/tool design, Claude Code configuration, prompt engineering, and context management -- with study prompts and hands-on build exercises for each domain.

## Topics
claude-code, agent-sdk, mcp, prompt-engineering, certification, multi-agent-orchestration
