# Reduce Hallucinations

**Author:** Anthropic
**Date:** 2026 (Anthropic Platform Docs)
**Source:** https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/reduce-hallucinations

## Key Takeaways

- Explicitly give Claude permission to say "I don't know" -- this single technique drastically reduces fabricated answers.
- For long documents (>20k tokens), ask Claude to extract word-for-word quotes before analysis to ground responses in actual source text.
- Make outputs auditable by requiring citations for every claim, and have Claude retract any claim it cannot back with a direct quote from the source.
- Chain-of-thought verification (step-by-step reasoning before the final answer) exposes faulty logic and hidden assumptions.
- Best-of-N verification (running the same prompt multiple times and comparing outputs) can surface inconsistencies that signal hallucinations.
- Restrict Claude to only use information from provided documents, explicitly barring its general knowledge, when factual precision matters.

## One-Line Summary

Anthropic's official guide to reducing hallucinations through quote-grounding, citation verification, permission to express uncertainty, and multi-pass consistency checks.

## Topics

hallucination reduction, prompt engineering, factual grounding, citation verification, Claude best practices, context engineering
