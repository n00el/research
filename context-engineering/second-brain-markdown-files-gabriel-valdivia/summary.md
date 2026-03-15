# I built a second brain out of markdown files

**Author:** Gabriel Valdivia (@gabrielvaldivia)
**Date:** March 11, 2026
**Source:** https://x.com/gabrielvaldivia/status/2031812714935328968

## Key Takeaways

- The entire system is a git repo of plain markdown files organized into folders (journal, digests, identity, people, clients, work, health, etc.) with Claude Code as the interface and 15 custom skills for reading/writing/cross-referencing data.
- Data flows in from 13+ sources (Gmail, Slack, iMessage, Beeper, Figma, Granola, Apple Health, Git, Substack, X, YouTube, Google Calendar, Slackdone) via MCP servers and local scripts, some live-queried and some synced on schedule.
- Three cron jobs drive the daily rhythm: morning briefing (7 AM), mid-day sync (12 PM), and full nightly digest (8 PM) that groups conversations by client across all channels rather than by platform.
- The digest's cross-referencing uses a two-pass verification to avoid false "open loops" -- it re-queries all outbound channels to check if you already responded before flagging something as needing follow-up.
- An "identity profile" that tracks beliefs, interests, communication patterns, and evolving thinking became an unexpected introspection tool -- surfacing conflicts and behavioral shifts before the author consciously recognized them.
- The key architectural insight is treating AI as the operator on plain-text files rather than building another app interface -- no vendor lock-in, fully portable, and readable by any future AI agent.
- The biggest unexpected value was not productivity but self-understanding: the system acts as a coach and accountability partner that surfaces patterns across all your communication channels.

## One-Line Summary

A markdown-and-git-based personal OS that pulls context from 13+ apps, uses Claude Code skills to synthesize daily digests and meeting prep, and unexpectedly became a powerful introspection tool by surfacing behavioral patterns across all communication channels.

## Topics

AI agents, personal knowledge management, second brain, Claude Code, MCP servers, markdown, productivity, introspection
