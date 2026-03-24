# How to build Claude Skills 2.0 Better than 99% of People

**Author:** leopardracer (@leopardracer)
**Date:** Mar 23, 2026
**Source:** https://x.com/leopardracer/status/2035999459729895493

## Key Takeaways

- Skills use a three-layer progressive disclosure structure (YAML metadata, body instructions, file references) to minimize token usage -- metadata loads at startup (dozens of tokens), full instructions only load when triggered.
- Keep SKILL.md under 500 lines; split detailed reference material into separate files that Claude loads on demand.
- Only include information Claude doesn't already know (company-specific rules, domain workflows, library quirks) -- skip general knowledge to save tokens.
- MCP defines what tools are available (the kitchen), Skills define how to use them (the recipes) -- combining both dramatically improves user experience.
- Use Skill-creator (a meta-skill) to iteratively generate, test, evaluate, and refine new skills without manual trial-and-error.
- Match instruction granularity to the task: high freedom for creative work, low freedom for critical procedures where consistency matters.
- Skills are best suited for repetitive, structured tasks (weekly reports, brand presentations, market analysis) -- one-off requests don't need skills.

## One-Line Summary

A comprehensive guide to Claude Skills 2.0 covering architecture (progressive disclosure, token efficiency), creation workflow (skill-creator meta-skill), best practices (concise instructions, MCP integration), and installation via the anthropics/skills marketplace.

## Topics

claude skills, skill creation, claude code, progressive disclosure, MCP integration, workflow automation, prompt engineering
