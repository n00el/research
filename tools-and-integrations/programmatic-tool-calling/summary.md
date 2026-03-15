# Give Claude a Computer

**Author:** Lance Martin (@RLanceMartin)
**Date:** Feb 27, 2026
**Source:** https://x.com/RLanceMartin/status/2027450018513490419

## Key Takeaways

- Programmatic Tool Calling (PTC) lets Claude write code that orchestrates tool calls inside a container, returning intermediate results to the running code rather than to Claude's context window, eliminating the per-call round-trip tax.
- The "composition tax" of traditional tool calling — latency, context bloat, and forced reasoning steps at every round trip — grows linearly with the number of actions and is the core problem PTC solves.
- PTC preserves the full control surface of tools (guardrails, observability, concurrency control, human approval) because tool handlers still sit on the host side of the sandbox boundary and intercept every call.
- Promoting an action to a tool is justified by five concerns: UX rendering, safety guardrails, concurrency control, observability, and autonomy level — not just "Claude needs to do X."
- On web search benchmarks (BrowseComp, DeepsearchQA), PTC improved accuracy by 11% while using 24% fewer input tokens, because code can filter and cross-reference results before they enter the context window.
- Opus 4.6 with PTC currently holds the #1 position on LMArena's Search Arena, demonstrating that code-orchestrated search meaningfully outperforms naive serial tool calling.

## One-Line Summary

Programmatic Tool Calling eliminates the latency and context bloat of serial tool round-trips by letting Claude orchestrate multi-step tool workflows in code, while keeping every call subject to host-side guardrails.

## Topics

programmatic tool calling, agent architecture, token efficiency, web search, claude api, tool orchestration
