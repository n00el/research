# Claude Health - Audit Your Claude Code Config Health

**Author:** tw93 (@AiTw93)
**Date:** Mar 23, 2026
**Source:** https://github.com/tw93/claude-health

## Key Takeaways

- A Claude Code skill that audits your project configuration across six layers: CLAUDE.md, rules, skills, hooks, subagents, and verifiers
- Detects project complexity tier (Simple/Standard/Complex) and calibrates checks accordingly -- won't flag missing layers for small projects
- Checks skill security for prompt injection, data exfiltration, destructive commands, and hardcoded credentials
- Monitors MCP overhead (flags if >12.5%), prompt cache invalidation patterns, and dangerous allowedTools entries
- Enforces "Three-Layer Defense" principle: critical rules should be covered by CLAUDE.md + Skill + Hook together
- Outputs a prioritized report at three severity levels: Critical (fix now), Structural (fix soon), Incremental (nice to have)
- Installable globally via `npx skills add tw93/claude-health -a claude-code -s health -g -y`

## One-Line Summary

A skill that runs a comprehensive health check on your Claude Code configuration across six architectural layers, detecting security issues, misconfigurations, and optimization opportunities.

## Topics

claude code, skills, configuration, security audit, developer tools, best practices
