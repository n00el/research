# Give Claude a Computer: Programmatic Tool Calling

**Author:** Lance Martin (@RLanceMartin), Anthropic
**Date:** February 27, 2026
**Source:** https://x.com/RLanceMartin/status/2027450018513490419
**Stats:** 1,283 likes, 149 retweets, 37 replies, 3,100 bookmarks, ~258K views

---

The post introduces and explains **Programmatic Tool Calling (PTC)**, a capability in Claude Opus 4.6 and Sonnet 4.6 that fundamentally changes how Claude interacts with tools. Instead of making individual tool calls that each round-trip through Claude's context window, Claude writes Python code that orchestrates multiple tool calls inside a code execution container.

## The Problem PTC Solves

Traditional tool calling creates two bottlenecks:
1. **Context pollution** from intermediate results flooding Claude's context window with data it doesn't ultimately need
2. **Inference overhead** from multiple round-trips to the model for each tool invocation

## How PTC Works

Claude generates orchestration code in Python. Tool calls execute within a code execution environment, and intermediate results stay in the code -- only final, filtered output enters Claude's context. Tools are marked with `allowed_callers: ["code_execution_20250825"]` to opt in.

## Benchmark Results

- On BrowseComp and DeepSearchQA (multi-step web research benchmarks), PTC delivered an **11% accuracy improvement** while using **24% fewer input tokens**.
- Claude Opus 4.6 with PTC ranked **#1 on LMArena's Search Arena**.
- In a budget compliance example, token consumption dropped from **43,588 to 27,297 tokens (37% reduction)**.

## When to Keep Discrete Tools vs. Code

- **UX considerations** for user-facing actions
- **Guardrails** for sensitive operations (e.g., file staleness checks)
- **Concurrency control** (grouping safe parallel operations)
- **Observability and logging** of specific actions
- **Autonomy-level grouping** for approval workflows

## Practical Example

Checking budget compliance across 20 employees -- the traditional approach requires 20 separate model round-trips pulling thousands of expense line items into context. With PTC, a single script runs all 20 lookups in parallel, filters results, and returns only the employees who exceeded limits.

## Part of Advanced Tool Use Suite

PTC is part of Anthropic's broader "Advanced Tool Use" suite that also includes:
- **Tool Search** -- on-demand tool discovery to reduce upfront token cost by ~85%
- **Tool Use Examples** -- concrete usage examples in tool definitions that improved accuracy from 72% to 90%

> "One tip for designing the agent action space: give Claude a computer and let it orchestrate tools in code."
