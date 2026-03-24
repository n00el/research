# 5 Claude Agents, 5 Discord Channels, 1 Obsidian Vault

**Author:** Artem Zhutov (@ArtemXTech)
**Date:** Mar 23, 2026
**Source:** https://x.com/ArtemXTech/status/2035933150186975304

## Key Takeaways

- Anthropic's new "Channels" feature lets you access your Claude Code session from your phone via Discord or Telegram, using your existing Obsidian vault context
- The key advantage over OpenClaw is that Channels is officially supported, requires no SSH maintenance, and uses Anthropic's subsidized $200/month subscription instead of expensive API keys
- Multi-agent Discord setup is possible: one bot token with an upgraded plugin routes different channels to separate Claude Code sessions (research, therapy, daily review, orchestrator, etc.)
- The orchestrator agent pattern enables cross-session visibility -- one agent can query what other agents are doing
- The /loop skill replicates OpenClaw's heartbeat functionality for periodic check-ins
- Context stored in an Obsidian vault (CLAUDE.md, memory files, skills) is what makes the agent personal rather than generic
- The combination of /loop, /dispatch, and channels now provides feature parity with OpenClaw for Claude Code users

## One-Line Summary

Anthropic's new Channels feature transforms Claude Code into a phone-accessible multi-agent system by routing Discord channels to separate Claude Code sessions running from an Obsidian vault.

## Topics

claude code, channels, discord, obsidian, multi-agent, personal assistant, openclaw, mobile access
