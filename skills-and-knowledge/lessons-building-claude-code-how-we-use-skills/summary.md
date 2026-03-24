# Lessons from Building Claude Code: How We Use Skills

**Author:** Thariq (@trq212)
**Date:** Mar 17, 2026
**Source:** https://x.com/trq212/status/2033949937936085378

## Key Takeaways

- Anthropic identifies 9 skill categories from hundreds in active use: Library/API Reference, Product Verification, Data Fetching, Business Process Automation, Code Scaffolding, Code Quality/Review, CI/CD, Runbooks, and Infrastructure Operations
- Skills are folders, not just markdown files -- use the file system for progressive disclosure with references/, assets/, scripts/ subdirectories that Claude reads on demand
- The highest-signal content in any skill is the Gotchas section, built up from common Claude failure points and updated over time
- The description field is not a summary but a trigger description -- it's what Claude scans to decide "is there a skill for this request?"
- On-demand hooks (e.g., /careful to block destructive commands, /freeze to restrict edits to specific directories) activate only when the skill is called
- Skills can store memory via log files, JSON, or SQLite in `$CLAUDE_PLUGIN_DATA` for persistence across upgrades; standup-post example keeps history to detect changes
- For distribution at scale, use a plugin marketplace with organic curation rather than checking all skills into repos (which adds context overhead)

## One-Line Summary

Anthropic's internal guide to building effective Claude Code skills, covering 9 skill categories, progressive disclosure via folder structure, gotchas sections, on-demand hooks, and marketplace distribution patterns.

## Topics

Claude Code, skills, agent engineering, Anthropic, developer tools, context engineering
