# Give Claude a Computer: Programmatic Tool Calling

**Author:** Lance Martin (@RLanceMartin), Anthropic
**Date:** February 27, 2026
**Source:** https://x.com/RLanceMartin/status/2027450018513490419

## Key Takeaways

- PTC lets Claude write Python code to orchestrate tool calls, keeping intermediate results out of the context window
- 11% accuracy improvement + 24% fewer input tokens on web research benchmarks; #1 on LMArena Search Arena
- Token consumption dropped 37% in a practical budget compliance example (43K → 27K tokens)
- Part of Anthropic's Advanced Tool Use suite alongside Tool Search (~85% token reduction) and Tool Use Examples (72% → 90% accuracy)
- Trade-off guidance: keep discrete tools for UX, guardrails, concurrency control, and observability needs

## One-Line Summary

Programmatic Tool Calling lets Claude orchestrate multiple tool calls via Python code execution, achieving better accuracy with fewer tokens by keeping intermediate results out of context.

## Topics

anthropic, claude, tool calling, programmatic tool calling, tokens, agent architecture
