# Self Improving Skills for Agents

**Author:** Vasilije (@tricalt)
**Date:** March 12, 2026
**Source:** https://x.com/tricalt/status/2032179887277060476

## Key Takeaways

- SKILL.md files are inherently static, but the environments they operate in (codebases, models, user tasks) constantly change, causing silent degradation over time.
- The cognee-skills approach closes the loop by treating skills as living system components with a structured lifecycle: ingest, observe, inspect, amend, evaluate.
- Every skill execution is observed and stored as a graph node, capturing what task was attempted, which skill ran, success/failure, errors, and user feedback.
- When failures accumulate, the system can inspect the connected execution history to trace root causes across runs, feedback, tool failures, and task patterns.
- Amendments to skills are proposed based on evidence from actual execution history, not guesswork -- they can tighten triggers, add conditions, reorder steps, or change output formats.
- A critical guardrail: every amendment must be evaluated against measurable improvement, with the ability to roll back if the new version does not outperform the old one.
- The full self-improvement cycle is: observe -> inspect -> amend -> evaluate, making skill evolution structured and auditable rather than uncontrolled.

## One-Line Summary

Cognee-skills introduces a closed-loop system where agent skills automatically detect their own degradation and propose evidence-based amendments, turning static SKILL.md files into self-improving components.

## Topics

AI agents, skill management, self-improving systems, cognee, prompt engineering, agent infrastructure
