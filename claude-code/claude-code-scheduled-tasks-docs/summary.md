# Run Prompts on a Schedule — Claude Code Scheduled Tasks

**Author:** Anthropic (Claude Code documentation)
**Date:** 2026
**Source:** https://code.claude.com/docs/en/scheduled-tasks

## Key Takeaways

- `/loop` command enables recurring prompts with configurable intervals (default 10min), supports looping over other skills
- One-time reminders via natural language ("remind me at 3pm to...") create single-fire cron tasks
- Session-scoped only — tasks are lost when Claude Code exits; use Desktop scheduled tasks or GitHub Actions for persistence
- 50-task limit per session with automatic 3-day expiry on recurring tasks
- Deterministic jitter prevents API thundering-herd; tasks fire between user turns at low priority
- All times use local timezone, not UTC

## One-Line Summary

Claude Code's `/loop` command and cron tools enable in-session recurring prompts and one-time reminders, but tasks don't survive restarts and expire after 3 days.

## Topics

claude code, scheduled tasks, cron, developer tools, automation, productivity
