# 8 Principles of Harness Engineering for AI Coding Agents

**Author:** Muratcan Koylan (@koylanai)
**Date:** March 5, 2026
**Source:** https://x.com/koylanai/status/2029427980699644360
**Stats:** 832 likes, 61 retweets, 25 replies, 1,567 bookmarks, ~110,800 views

---

Koylan distills eight key principles from an OpenAI blog post about "harness engineering" -- the practice of designing systems and environments so that AI coding agents (specifically OpenAI's Codex) can operate effectively. The framing shifts the engineer's role from writing code to designing the scaffolding and infrastructure that agents work within.

## 8 Principles

1. **Engineers become environment designers.** The focus shifts from incremental coding effort to designing the overall system architecture that agents operate in.

2. **Progressive disclosure for documentation.** Rather than monolithic docs, use a concise index that links to organized, layered reference materials -- making it easier for agents to navigate.

3. **Repository-encoded, version-controlled information.** Agent utility depends on accessibility; all relevant information must live in the repo and be version-controlled so agents can find and use it.

4. **Mechanical enforcement over instructions.** Custom linters and automated checks are more effective than writing guidelines or instructions for agents to follow. Enforce correctness structurally.

5. **Composable, stable APIs with strong training-set representation.** Agents work best with well-known, stable APIs that are well-represented in their training data.

6. **Automated background agents for self-correction.** Deploy agents that run in the background to detect pattern deviations and self-correct, rather than relying solely on human oversight.

7. **Velocity-first merge philosophy.** Prioritize merging speed; address test failures post-merge rather than using them as blockers. This keeps the development loop fast.

8. **Agent-to-agent code review.** Code review shifts from human-driven to agent-to-agent, with human judgment reserved only for edge cases.

## Notable Insights

- The concept of "harness engineering" represents a paradigm shift: engineers no longer primarily write code but instead design the environments, tooling, and constraints within which AI agents produce code.
- The emphasis on **mechanical enforcement** (linters, automated checks) over natural-language instructions is a pragmatic acknowledgment that agents follow structural constraints more reliably than written guidelines.
- The **post-merge test failure** approach is a notable departure from traditional CI/CD philosophy, trading safety for velocity in an agent-driven workflow.
