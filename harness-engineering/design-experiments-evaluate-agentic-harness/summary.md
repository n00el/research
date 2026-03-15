# How to design Experiments to Evaluate your Agentic Harness

**Author:** AVB (@neural_avb)
**Date:** March 10, 2026
**Source:** https://x.com/neural_avb/status/2031417353666441266

## Key Takeaways

- Evaluation is not just logging metrics on your current system -- it includes running controlled experiments to validate assumptions and compare alternative approaches (e.g., different models, prompts, tools).
- Treat each agent in a multi-agent pipeline as a separate evaluation harness; avoid evaluating the entire pipeline at once because too many variables make results unactionable.
- Isolate your module as a pure black-box function with explicit "knobs" (independent variables like model name, prompt template, feature flags) and disable caching/persistence to ensure repeatable, transactional test cases.
- Use real production logs as test cases whenever possible; if you don't have logs, set up analytics first before attempting evaluation.
- Prefer deterministic eval metrics (precision, recall, IOU, regex format checks) over LLM-as-judge because they are cheaper, faster, and 100% reliable. Always record walltime, token usage, and cost alongside quality metrics.
- The author replaced gpt-5-mini with gemini-3-flash-lite for their retrieval subagent, achieving lower cost and faster latency while maintaining accuracy -- a finding only possible through systematic experimentation.
- Evaluation harnesses are structurally similar to RL environments, opening the door to prompt optimization (GEPA) or end-to-end RL training on your own harnesses.

## One-Line Summary

A practical 6-step framework for designing controlled experiments to evaluate individual agents in your pipeline, with real-world examples from a retrieval subagent where systematic testing led to a cheaper and faster model replacement.

## Topics

agentic evaluation, experiment design, harness engineering, retrieval agents, model comparison, cost optimization
