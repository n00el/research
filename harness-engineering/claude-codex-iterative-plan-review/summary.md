# I Made Claude and Codex Argue Until My Code Plan Was Actually Good

**Author:** Aseem Shrey
**Date:** February 20, 2026
**Source:** https://aseemshrey.in/blog/claude-codex-iterative-plan-review/

## Key Takeaways

- Single-model planning produces coherent but unchallenged output; using a second model as an adversarial reviewer catches architectural blind spots, security gaps, and edge cases the planner misses.
- The iterative loop (plan, review, revise, re-review) is superior to one-shot review because it verifies that fixes actually resolve the issues without introducing regressions.
- Codex CLI's session resume feature (`codex exec resume <session-id>`) preserves context across rounds, so the reviewer remembers prior feedback and checks whether issues were addressed rather than re-discovering them.
- The skill is implemented as a single SKILL.md file with no external infrastructure -- it orchestrates Claude Code's tool execution and Codex CLI's session management via a structured VERDICT protocol (APPROVED/REVISE) to control the loop.
- This approach is best reserved for high-stakes plans (auth, data models, concurrency, multi-day implementations) and should be skipped for simple bug fixes where speed matters more than thoroughness.
- In a real-world test, 3 automated rounds caught 14 issues (broken auth, shell quoting bugs, schema conflicts, missing concurrency handling) and elevated a rough draft to a production-grade spec with zero manual review effort.

## One-Line Summary

Pitting two frontier models against each other in a structured review loop catches far more plan-level defects than any single-model workflow, turning rough drafts into production-ready specs without manual effort.

## Topics

claude code skills, adversarial ai review, codex cli, implementation planning, multi-model workflows, developer tooling
