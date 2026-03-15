# Code Mode: Give Agents an Entire API in 1,000 Tokens

**Author:** Matt Carey
**Date:** February 20, 2026
**Source:** https://blog.cloudflare.com/code-mode-mcp/

---

## Overview

Cloudflare has introduced a new MCP (Model Context Protocol) server that revolutionizes how AI agents access large APIs. The innovation centers on "Code Mode," a technique that dramatically reduces token consumption by allowing models to write code against typed SDKs rather than exposing thousands of individual tools.

## The Core Problem and Solution

The fundamental challenge with MCP is that "agents need many tools to do useful work, yet every tool added fills the model's context window." The Cloudflare API alone contains over 2,500 endpoints -- a traditional MCP implementation would consume approximately 1.17 million tokens.

Code Mode solves this by collapsing the entire API into just two tools: `search()` and `execute()`. This approach reduces token usage by 99.9% while maintaining full API coverage.

## How Code Mode Works

### Server-Side Architecture

Both tools operate within a Dynamic Worker isolate, a lightweight V8 sandbox that prevents security risks:

- **`search()` tool:** Allows agents to query the OpenAPI specification using JavaScript to discover relevant endpoints without loading the full spec into context
- **`execute()` tool:** Enables agents to write code that makes authenticated Cloudflare API requests, handles pagination, and chains multiple operations

The sandbox features include no file system access, blocked environment variable leakage, and disabled external fetches by default.

### Practical Example: DDoS Protection

The article illustrates the workflow through a scenario where an agent protects an origin from DDoS attacks:

**Step 1 - Search:** The agent writes JavaScript to filter the spec for WAF and ruleset endpoints on a zone, narrowing thousands of endpoints to the handful needed.

**Step 2 - Execute:** The agent chains API calls to check existing rulesets, inspect their configurations, and update sensitivity levels in a single execution.

This entire operation required only four tool calls.

## Key Advantages

- **Fixed token footprint:** Regardless of API growth, the cost remains approximately 1,000 tokens
- **Progressive capability discovery:** Agents explore APIs dynamically without predefined tool sets
- **Security:** Code executes in isolated Workers environments with explicit permission controls
- **OAuth 2.1 compliance:** Uses Workers OAuth Provider to downscope tokens to user-approved permissions
- **Scalability:** New API endpoints automatically discoverable without creating new MCP servers

## Context Reduction Approaches

The article compares Code Mode with alternative strategies:

**Client-side Code Mode** requires secure sandbox access on the client but offers flexibility for SDK-based development. **Command-line interfaces** provide self-documentation through progressive disclosure but introduce broader security surfaces. **Dynamic tool search** shrinks context but requires maintained search functions.

Server-side Code Mode combines these strengths while avoiding their drawbacks.

## Availability and Setup

The Cloudflare MCP server is available now. Implementation requires adding a single configuration:

```json
{
  "mcpServers": {
    "cloudflare-api": {
      "url": "https://mcp.cloudflare.com/mcp"
    }
  }
}
```

Users authenticate through OAuth 2.1 or can provide API tokens directly for CI/CD environments.

## Open Source Contributions

Cloudflare has open-sourced the Code Mode SDK within the Cloudflare Agents SDK, enabling developers to implement similar patterns in their own MCP servers.

## Future Direction

The organization plans MCP Server Portals that compose multiple MCP servers behind a unified gateway with integrated Code Mode functionality, addressing scenarios where agents interact with multiple services simultaneously.
