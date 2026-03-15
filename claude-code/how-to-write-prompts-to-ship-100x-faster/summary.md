# How to write Prompts to ship 100x faster

**Author:** Himanshu (@nothiingf4)
**Date:** March 8, 2026
**Source:** https://x.com/nothiingf4/status/2030682331670056964

## Key Takeaways

- Start every prompt with the 3 W's (What is the task, Who is the audience, What format/outcome) to narrow the model's probability space and get focused outputs.
- Be specific rather than vague -- specificity reduces entropy in token prediction, meaning fewer valid next tokens and more accurate outputs.
- Break complex tasks into numbered step-by-step instructions that act as sequential processing checkpoints, preventing the model from hallucinating shortcuts.
- Explicitly define the output format (JSON, bullet points, markdown template) to act as a structural filter during token scoring.
- Use Chain-of-Thought (CoT) prompting by asking the model to reason before answering -- even just appending "think step by step" significantly improves accuracy on reasoning tasks.
- Use few-shot examples and explicit constraints ("under 100 words", "no jargon") as guard rails that prune the model's output candidates during generation.
- The best prompts layer multiple tactics together (context + steps + format + examples + specificity) -- they are complementary ingredients, not alternatives.

## One-Line Summary

A practical 7-tactic framework for writing effective LLM prompts, grounded in how token probability works, with ready-to-use templates for code review, writing, decision-making, and debugging.

## Topics

prompt engineering, LLM, Claude Code, developer productivity, AI coding, chain-of-thought, few-shot prompting