# Claude Subagents vs. Agent Teams, Explained

**Author:** Akshay (@akshay_pachaar)
**Date:** March 15, 2026
**Source:** https://x.com/akshay_pachaar/status/2033167408463069526

## Key Takeaways

- Claude offers two distinct multi-agent paradigms -- sub-agents (isolated, fire-and-forget workers that return compressed results) and agent teams (long-running instances with peer-to-peer communication and shared task state). They solve fundamentally different coordination problems.
- Sub-agents excel at parallelism through isolation: each gets its own context window, tools, and single job. Only the final result returns to the parent, keeping the parent's context clean. They cannot spawn children or talk to siblings.
- Agent teams support ongoing coordination: teammates persist, accumulate context, communicate directly with each other, and use a shared task list with dependency tracking (e.g., blockedBy fields).
- The key design decision is context-centric decomposition -- split work by what context each subtask needs, not by role (planner/implementer/tester). Role-based splits create a "telephone game" where information degrades at every handoff.
- Most people reach for multi-agent systems too early. Better prompting on a single agent often achieves equivalent results. Multi-agent architectures earn their cost only for context protection, true parallelization, or conflicting specialization requirements.
- Five orchestration patterns cover most real-world needs: prompt chaining, routing, parallelization, orchestrator-worker, and evaluator-optimizer.
- Common failure modes: vague task descriptions causing duplicated work, verification agents that don't actually verify, and token costs compounding faster than expected. Mitigation requires explicit task boundaries, concrete acceptance criteria, and intelligent model tiering.

## One-Line Summary

Most people reach for multi-agent systems too early -- Claude's sub-agents (isolated, fire-and-forget) and agent teams (persistent, communicating) solve different problems, and the right architecture comes from splitting by context boundaries rather than roles.

## Topics

multi-agent systems, sub-agents, agent teams, Claude SDK, orchestration patterns, context-centric decomposition, agent architecture, parallelism, coordination, token cost management
