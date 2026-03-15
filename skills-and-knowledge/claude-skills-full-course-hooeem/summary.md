# I want to learn how to use Claude Skills (full course)

**Author:** hoeem (@hooeem)
**Date:** March 11, 2026
**Source:** https://x.com/hooeem/status/2031755971265974632

## Key Takeaways

- A Claude Skill is just a folder with a SKILL.md file dropped into `~/.claude/skills/` -- the YAML `description` field is the single most important line because it controls when the skill activates
- Skills handle procedural instructions (how to do tasks), Projects handle knowledge (reference material), and MCP handles live data connections -- they serve distinct purposes
- The five-step build process: define the job with ruthless specificity, write "pushy" YAML triggers with explicit phrases and negative boundaries, write sequential instructions with concrete examples, organize references one level deep, then deploy
- Use scripts/ for computation-heavy tasks while keeping judgment and language tasks in the instructions -- they complement each other inside a single skill
- Multi-skill conflicts are solved by non-overlapping territories, aggressive negative boundaries in YAML descriptions, and distinctive trigger language for each skill
- Every skill failure falls into five diagnosable categories: Silent (never fires), Hijacker (fires on wrong requests), Drifter (wrong output), Fragile (breaks on edge cases), Overachiever (adds unrequested content)
- For long-running projects, add a "shift handover" pattern: read context-log.md at session start, write a summary at session end, so Claude picks up where it left off

## One-Line Summary

A structured 4-module course that takes you from zero to production-ready Claude Skills, with copy-paste prompts for each step and practical patterns for testing, debugging, and multi-skill orchestration.

## Topics

claude skills, claude code, AI automation, prompt engineering, developer tools, productivity
