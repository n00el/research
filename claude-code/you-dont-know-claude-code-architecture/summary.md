# You Don't Know Claude Code: Architecture, Governance, and Engineering Practices

**Author:** Tw93 (@HiTw93)
**Date:** March 12, 2026
**Source:** https://x.com/HiTw93/status/2033181380432339045

## Key Takeaways

- Claude Code architecture is best understood as six interdependent layers (CLAUDE.md/rules/memory, Tools/MCP, Skills, Hooks, Subagents, Verifiers) -- over-indexing on one layer destabilizes the system.
- Context engineering is the most critical skill: a typical 200K context window leaves only ~160-180K tokens for actual work after fixed overhead from system prompts, tool definitions, and CLAUDE.md.
- CLAUDE.md should be a compact operational contract (build commands, constraints, prohibitions), not a wiki -- move reference materials to skills and supporting files.
- Skills should follow three patterns (checklists, workflows, domain experts) with brief descriptors (~9-10 tokens) to minimize context consumption while enabling on-demand loading.
- Hooks provide deterministic enforcement (file protection, auto-formatting, context injection) and save cumulative hours in long editing sessions by catching errors early.
- Prompt caching is an architectural constraint, not just cost optimization -- switching models mid-session rebuilds the entire cache and can make "cheaper" models more expensive.
- The fundamental verification test: "If you cannot clearly explain how Claude knows it's done correctly, it's probably not suitable for autonomous completion."

## One-Line Summary

A practitioner's handbook presenting Claude Code as a six-layer engineering system where context management, deterministic hooks, skill design, and verification loops matter more than raw model capability.

## Topics

claude-code, context-engineering, CLAUDE.md, skills, hooks, prompt-caching, verification, agent-architecture
