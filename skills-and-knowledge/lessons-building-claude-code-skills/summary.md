# Lessons from Building Claude Code: How We Use Skills

**Author:** Thariq (@trq212)
**Date:** 2026-03-17
**Source:** https://x.com/trq212/status/2033949937936085378

## Key Takeaways

- Skills are folders, not just markdown files -- the most powerful skills use folder structure (scripts, assets, references) as progressive disclosure for context engineering
- Anthropic categorizes their hundreds of internal skills into 9 types: Library/API Reference, Product Verification, Data Fetching, Business Process Automation, Code Scaffolding, Code Quality, CI/CD, Runbooks, and Infrastructure Operations
- The Gotchas section is the highest-signal content in any skill -- build it up from real failure points over time
- Skills can register on-demand hooks (e.g., /careful to block destructive commands, /freeze to limit edits to a directory) that activate only when invoked
- Use `${CLAUDE_PLUGIN_DATA}` for persistent storage that survives skill upgrades; skill-directory data may be deleted on update
- The description field is a trigger condition for the model, not a human-readable summary -- write it to help Claude decide when to invoke the skill
- For distribution at scale, internal plugin marketplaces beat repo-checked skills because they reduce context overhead and let teams curate independently

## One-Line Summary

Anthropic's internal playbook for building, structuring, and distributing Claude Code skills, organized into 9 skill categories with concrete tips on progressive disclosure, gotchas sections, on-demand hooks, and marketplace curation.

## Topics

claude code, skills, agent engineering, plugin marketplaces, context engineering, progressive disclosure
