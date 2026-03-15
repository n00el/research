# Give Claude a Computer

**Author:** Lance Martin (@RLanceMartin)
**Date:** Feb 27, 2026
**Stats:** 34.6K views | 351 likes | 41 reposts | 671 bookmarks
**URL:** https://x.com/RLanceMartin/status/2027450018513490419

---

## TL;DR

Programmatic Tool Calling (PTC) is a capability in Claude Opus/Sonnet 4.6. Instead of making tool calls that each round-trip through Claude's context, Claude writes code that orchestrates tool calls directly inside a container. Intermediate tool results return to the code, not Claude's context window. This reduces token usage and improves performance on multi-step tasks like search. Opus 4.6 with PTC scored #1 on LMArena's Search Arena benchmark.

## Tools Primer

Actions are how Claude interacts with the world. Tools declaratively specify the actions Claude can take — a tool name, description, and input arguments. If Claude wants to call a tool, it responds with a JSON object of arguments. A tool handler (MCP server, custom code, etc.) runs the tool and passes back context. Running this in a loop = an agent.

Example — bash tool produces:
```json
{"command": "pip install requests"}
```

## When to Promote an Action to a Tool

Claude with a bash tool in a loop is a computer-use agent (central to Claude Code). But Claude Code doesn't *just* use bash — it uses tools as a **control surface** around certain actions. Reasons to promote an action to a tool:

- **UX:** Some actions need to be caught and rendered to the user in a particular way (e.g., AskUserQuestion tool)
- **Guardrails:** Some actions need safety checks (e.g., file edit tool with staleness verification)
- **Concurrency control:** Group actions by concurrency safety (read-only tools run in parallel)
- **Observability:** Isolate specific actions for logging (latency, token usage)
- **Autonomy:** Group actions by autonomy level — if the harness can undo an action, it can approve more freely

## The Problem with Tools

Tools trade off control with composability. Consider three sequential tool calls:
- Context from each call returns back to Claude
- Each round trip costs latency
- Serializes full tool result into context (thousands of rows even if next step only needs five)
- Introduces a reasoning step each time
- The **composition tax** grows with the number of actions

## Programmatic Tool Calling (PTC)

PTC unites the composability of code with the control surface of tools:

1. You define tools as usual
2. Instead of calling them individually, Claude **composes them as functions** and runs them in a code execution container
3. Tool results **return to the container** (the running code) rather than to Claude's context window

### How It Works

When the code calls a tool (e.g., `await web_search(query)`):
1. Container pauses
2. Call crosses sandbox boundary as a typed tool-use event
3. Fulfilled just as if the model directly called the tool (via handler, MCP server, etc.)
4. Result returns **to the running code**, not Claude's context
5. Code processes it following Claude's specified control flow (calls another tool, filters data, accumulates results)
6. Only the **final output** reaches Claude

### Performance Results

With Opus 4.6, gains in token efficiency and performance on non-coding evals:
- **BrowseComp** and **DeepsearchQA** benchmarks for web search
- Improved accuracy by average of **11%**
- Used **24% fewer input tokens**
- Opus 4.6 with PTC is currently **#1 in LMArena's Search Arena**

Rather than pulling 50 raw search results into context, the code can parse, filter, and cross-reference results programmatically — keeping what's relevant, discarding the rest (dynamic filtering).

### Using PTC with Web Search (API)

PTC is built into the web search tool on the API:

```json
{
  "model": "claude-opus-4-6",
  "max_tokens": 4096,
  "tools": [
    {
      "type": "web_search_20260209",
      "name": "web_search"
    }
  ],
  "messages": [
    {
      "role": "user",
      "content": "<query>"
    }
  ]
}
```

## Key Takeaway

PTC gets the benefit of code execution (composability) while preserving the control surface of tools: tool implementations run on your side of the sandbox, not inside it. Tool handlers still sit in the middle of every call, able to inspect, reject, log, or queue for human approval. But it allows Claude to fluently orchestrate actions in code.
