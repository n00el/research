# Claude Code Cheat Sheet - Full Reference Guide

**Author:** Om Patel (@om_patel5)
**Date:** March 8, 2026
**Source:** https://x.com/om_patel5/status/2030676523661901889

## Key Takeaways

- Claude Code has three permission modes (Normal, Auto-Accept, Plan) cycled with Shift+Tab -- starting in Plan Mode then switching to Normal/Auto-Accept is the recommended workflow.
- The "Big 5" extension system (CLAUDE.md, Custom Commands, Skills, Sub-Agents, MCP Servers) makes Claude Code a fully programmable development environment, not just a chat tool.
- Hooks (PreToolUse, PostToolUse, SessionStart, etc.) enable event-driven automation around Claude, such as auto-formatting after edits or blocking writes to sensitive files.
- Session management is more powerful than most realize: `/rewind` restores both conversation and code state, `/compact` compresses context with optional hints, and `claude -r` resumes named sessions.
- CLI piping (`git diff | claude -p "review this"`) and flags like `--max-budget-usd` and `--tools` give fine-grained control for scripting and CI integration.
- Custom Slash Commands (you invoke) vs Skills (Claude auto-invokes) vs Sub-Agents (separate AI instances) serve distinct purposes and can be combined for complex workflows.

## One-Line Summary

A comprehensive 2026 reference covering every Claude Code keyboard shortcut, slash command, CLI flag, hook, permission mode, and extension mechanism, revealing it as a full AI-powered development operating system rather than a simple chat tool.

## Topics

claude code, developer tools, AI coding, cheat sheet, productivity, CLI tools
