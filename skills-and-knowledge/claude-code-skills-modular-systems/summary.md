# How Anthropic Engineers Actually Use Skills Inside Claude Code

**Author:** Prakash Sharma (@PrakashS720)
**Date:** Mar 21, 2026
**Source:** https://x.com/PrakashS720/status/2035400340988944522

## Key Takeaways

- Skills are modular systems (folders, not files) that agents explore and execute, not just text to read
- Top teams organize skills into clear categories: knowledge, verification, data, automation, scaffolding, review, CI/CD, runbooks, and infra ops
- The biggest unlock is verification -- building systems that simulate real usage and run assertions, not just generating output
- Skills should evolve by capturing edge cases, failures, and "gotchas" so every mistake becomes part of the system
- Progressive disclosure through folder-based skills lets the filesystem become part of the agent's brain
- The best approach is providing structure and high-signal context while allowing flexibility, rather than rigid over-constrained prompts
- Skills should be treated as internal products: reusable, composable, and shareable across the organization

## One-Line Summary

Anthropic engineers treat Claude Code skills as modular, category-organized systems with single responsibilities -- where verification and progressive disclosure matter far more than rigid prompt engineering.

## Topics

claude code, skills, agent design, verification, progressive disclosure, modular systems
