# You Don't Know AI Agents: Principles, Architecture, and Engineering Practices

**Author:** Tw93 (@HiTw93)
**Date:** Mar 22, 2026
**Source:** https://x.com/HiTw93/status/2035527178419683540

## Key Takeaways

- The agent loop core is stable (~20 lines of code); new capabilities should be layered outside it through tool expansion, prompt structuring, and state externalization -- never by turning the loop into a state machine
- Harness quality (acceptance baselines, execution boundaries, feedback signals, fallbacks) determines agent stability far more than model choice -- fix the eval before tuning the agent
- Context Rot is the most common failure mode; prevent it with layered context (permanent, on-demand, runtime, memory, system layers) and cache-friendly prompt design where stable prefixes get 90% cost discounts
- Tool design must follow ACI (Agent-Computer Interface) principles: map to agent goals not API endpoints, provide structured errors with recovery suggestions, and include 1-5 usage examples to boost accuracy from 72% to 90%
- Memory splits into four types (working, procedural, episodic, semantic) with reversible consolidation -- the system moves pointers without deleting raw messages so failures don't lose context
- Multi-agent systems require protocols before collaboration: JSONL inbox communication, worktree isolation, task graph dependencies, and sub-agents that return only summaries
- Security boundaries must precede features: whitelist authorization, workspace path isolation, audit logging, untrusted content wrapping, and provider fallback chains

## One-Line Summary

A comprehensive engineering guide arguing that agent stability comes not from better models but from disciplined harness engineering, layered context management, ACI-principled tool design, reversible memory consolidation, and strict security boundaries -- illustrated with OpenClaw's production implementation.

## Topics

agent architecture, context engineering, tool design, memory systems, multi-agent, evaluation, harness engineering
