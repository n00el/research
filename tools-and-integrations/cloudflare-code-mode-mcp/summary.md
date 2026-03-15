# Code Mode: Give Agents an Entire API in 1,000 Tokens

**Author:** Matt Carey
**Date:** February 20, 2026
**Source:** https://blog.cloudflare.com/code-mode-mcp/

## Key Takeaways

- Exposing large APIs as individual MCP tools is unsustainable -- Cloudflare's 2,500+ endpoints would consume ~1.17 million tokens in a traditional tool-per-endpoint approach.
- Code Mode collapses an entire API into two tools (`search()` and `execute()`), cutting token usage by 99.9% to a fixed ~1,000-token footprint regardless of API size.
- Agents dynamically discover endpoints by writing JavaScript that queries the OpenAPI spec at runtime, eliminating the need for predefined tool sets.
- The `execute()` tool runs agent-authored code inside a V8 Worker isolate (no filesystem, no env var leakage, no external fetch by default), making sandboxed execution practical for production use.
- New API endpoints become automatically discoverable without creating or registering additional MCP servers, solving the maintenance scaling problem.
- Cloudflare open-sourced the Code Mode SDK inside their Agents SDK, so third-party developers can apply the same pattern to their own APIs.
- The next step is MCP Server Portals -- a unified gateway that composes multiple MCP servers behind a single Code Mode interface for multi-service agent workflows.

## One-Line Summary

Cloudflare's Code Mode replaces thousands of individual MCP tools with two sandbox-executed code tools that let agents programmatically explore and call any API endpoint at a fixed token cost.

## Topics

mcp, ai agents, token optimization, cloudflare workers, api design, developer tooling
