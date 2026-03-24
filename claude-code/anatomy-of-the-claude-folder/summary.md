# Anatomy of the .claude/ Folder

**Author:** Akshay (@akshay_pachaar)
**Date:** Mar 21, 2026
**Source:** https://x.com/akshay_pachaar/status/2035341800739877091

## Key Takeaways

- There are two `.claude` directories: one project-level (committed, shared with team) and one global at `~/.claude/` (personal preferences, session history, auto-memory).
- `CLAUDE.md` is the highest-leverage file -- keep it under 200 lines with build commands, architecture decisions, conventions, and gotchas; avoid duplicating linter configs or full docs.
- The `rules/` folder lets you split instructions by concern (code-style, testing, API conventions) and scope them to specific file paths via YAML frontmatter, so rules only activate for relevant files.
- Custom slash commands in `commands/` can embed live shell output using `!` backtick syntax and accept `$ARGUMENTS`, making them dynamic rather than just saved text.
- Skills differ from commands: they auto-trigger based on conversation context matching the skill's description, and they can bundle supporting files as packages rather than being single files.
- Agents in `agents/` are subagent personas with their own model, tool restrictions, and isolated context windows -- useful for delegating focused tasks like code review to cheaper/faster models.
- `settings.json` defines allow/deny permission lists with an intentional middle ground: unlisted commands prompt for confirmation, providing safety without requiring exhaustive upfront configuration.

## One-Line Summary

A comprehensive walkthrough of every file and folder in the `.claude/` directory, explaining how CLAUDE.md, rules, commands, skills, agents, and settings.json work together as a protocol for controlling Claude Code's behavior in your project.

## Topics

claude code, CLAUDE.md, developer tools, project configuration, skills, agents, custom commands
