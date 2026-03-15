# How to Force Your Agent to Obey Your Design System

**Author:** Ryan Carson (@ryancarson)
**Date:** March 3, 2026
**Source:** https://x.com/ryancarson/status/2028916090596643078

## Key Takeaways

- Design systems fail because they're optional — treat them as enforceable contracts instead
- 5-layer approach: canonical docs → agent routing (SKILL.md) → custom lint rules → pre-commit hooks → CI gates
- Custom lint rules ban raw values (hex colors, spacing utilities) and require semantic tokens
- Single orchestrator command (`lint:design-system`) for both pre-commit and CI
- Framework-agnostic and agent-agnostic — works with any stack and any coding agent
- High bookmark count (2,369) signals strong practical/reference value

## One-Line Summary

A 5-layer deterministic system (docs, agent routing, custom lints, pre-commit hooks, CI gates) that treats design systems as enforceable contracts rather than optional guidance for AI coding agents.

## Topics

design systems, AI agents, linting, CI/CD, developer workflow, frontend
