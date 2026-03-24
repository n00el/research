# This SIMPLE Obsidian + Claude Code setup could turn your vault into a 24x7 AI agent

**Author:** Kanika (@KanikaBK)
**Date:** Mar 23, 2026
**Source:** https://x.com/KanikaBK/status/2036117598132441196

## Key Takeaways

- Connect Obsidian to Claude Code via the Local REST API plugin, which exposes read/write endpoints on localhost for your vault.
- Create a simple `/obsidian` skill file (~/.claude/commands/obsidian.md) that teaches Claude how to call the local API to search, read, and write notes.
- Three scheduling layers for automation: /loop (session-based, same-day), Claude Desktop scheduled tasks (persistent, survives restarts), and VPS/cron (truly 24x7).
- Claude Desktop scheduled tasks are the sweet spot for most users -- they spin up a fresh Claude Code session at a set time daily without needing a terminal open.
- For true always-on operation, deploy Claude Code on a VPS with systemd/pm2 and sync the Obsidian vault via Git.
- The end result transforms a static Obsidian vault into a living system that auto-generates daily briefings, extracts todos, and maintains notes autonomously.

## One-Line Summary

Step-by-step guide to wiring Obsidian's Local REST API plugin with a Claude Code custom skill and scheduled tasks to create an autonomous 24x7 AI agent that maintains your knowledge base.

## Topics

obsidian, claude code, AI agent, automation, scheduling, knowledge management, skills
