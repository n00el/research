# Summary: The Missing Piece in Every Claude Code Setup

## Key Takeaways

- **OAuth token refresh in Claude Code is broken**: Claude Code stores OAuth refresh tokens in macOS Keychain but doesn't actually use them to refresh, leading to repeated re-authentication throughout the day -- a known bug open for months.
- **Credential sprawl is a real security risk**: Over 30,000 publicly exposed OpenClaw instances were found by Bitsight, and 7.1% of ClawHub skills leak credentials according to Snyk.
- **Composio provides centralized auth for AI agents**: It routes authentication through managed OAuth flows with encrypted storage, scoped permissions, and automatic token refresh (SOC 2 and ISO 27001 compliant).
- **1,000+ agent-first toolkits**: Composio covers business apps like Salesforce, HubSpot, Jira, Google Workspace, Datadog, PagerDuty, Notion, and Confluence -- many without first-party MCP integrations.
- **Just-in-time tool discovery**: Instead of preloading hundreds of tool definitions into context, Composio uses 5 meta-tools that discover tools on demand, preserving the agent's token budget for reasoning.
- **Setup is minimal**: A single MCP URL in config or a few CLI commands (install, login, link) handle all authentication with no tokens stored in config files.
- **Composable multi-app workflows**: Once auth is solved, agents can orchestrate across multiple services (YouTube + Slack, HubSpot + Gmail, Datadog + PagerDuty) with a single auth layer.

## One-Line Summary

Composio solves the persistent OAuth re-authentication problem in Claude Code and OpenClaw by providing centralized, encrypted credential management with 1,000+ agent-first toolkits and just-in-time tool discovery.

## Topics

- Claude Code, OpenClaw, MCP, OAuth, authentication, token refresh, Composio, agent tooling, credential management, security, multi-app workflows, CLI, just-in-time discovery
