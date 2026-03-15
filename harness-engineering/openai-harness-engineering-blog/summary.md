# Harness Engineering: Leveraging Codex in an Agent-First World

**Author:** OpenAI
**Date:** February 2026
**Source:** https://openai.com/index/harness-engineering/

## Key Takeaways

- 3 engineers shipped ~1M lines of 100% agent-generated code in 5 months via ~1,500 PRs (~3.5 PRs/engineer/day)
- AGENTS.md should be a short table of contents, not a monolith; use progressive disclosure via structured docs/
- Strict layered architecture (Types→Config→Repo→Service→Runtime→UI) enforced by structural tests, not conventions
- Custom linter error messages are designed to inject remediation instructions into the agent's context
- Repository optimized for agent legibility first, human readability second
- Background Codex tasks continuously scan for deviations and open targeted refactoring PRs

## One-Line Summary

OpenAI's "harness engineering" discipline shows that the moat in AI-driven development is the control infrastructure — linters, CI, docs structure, and feedback loops — not the code itself.

## Topics

harness engineering, openai, codex, agentic coding, CI/CD, developer workflow, architecture
