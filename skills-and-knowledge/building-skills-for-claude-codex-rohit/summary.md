# Summary: The Complete Guide to Building Skills for Claude/Codex

## Key Takeaways

- **Skills are the new standard for AI agents:** Launched by Anthropic in October 2025, skills have evolved from a proprietary feature into an open standard adopted by OpenAI, Microsoft, and others. They are dynamic, organized packages that load context on demand -- not just static prompts.
- **Three-level progressive disclosure architecture:** Skills use a layered system: (1) YAML frontmatter always loaded for routing decisions, (2) SKILL.md body loaded when relevant, (3) linked resources (scripts, references, assets) loaded as needed -- minimizing token waste while enabling rich instructions.
- **The description field is critical for triggering:** A skill's description determines when Claude loads it. Effective descriptions include trigger phrases, relevant file types, and clear problem statements. Poor descriptions lead to missed activations or false positives.
- **skills.sh CLI is the "npm for AI agents":** Vercel's CLI tool (early 2026) enables installing, managing, and updating skills across 35+ agents (Claude Code, Cursor, Codex, Windsurf, etc.) with a marketplace and popularity rankings.
- **Three main skill categories:** Document/asset creation, workflow automation, and MCP enhancement. The most powerful skills orchestrate multi-MCP workflows (e.g., Figma -> Google Drive -> Linear -> Slack).
- **Security requires trust verification:** Skills can execute code and invoke tools, so Anthropic recommends only using skills from trusted sources (Anthropic-created, self-created, or verified partners). Community skills should be reviewed before installation.
- **Skills as competitive advantage:** Organizations building internal skill repositories gain measurable productivity advantages. Tasks that took 30+ minutes with generic AI complete in under 3 minutes with proper skills.

## One-Line Summary

A comprehensive technical guide explaining the agent skills standard -- from architecture and progressive disclosure to building, testing, distributing, and securing skills for Claude Code and other AI coding agents.

## Topics

skills, claude-code, codex, SKILL.md, agent-architecture, progressive-disclosure, skills.sh, MCP, prompt-engineering, open-standard, security, workflow-automation, vercel, anthropic
