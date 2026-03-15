# Recursive Language Models

**Authors:** Alex L. Zhang, Tim Kraska, Omar Khattab
**Date:** January 29, 2026
**Source:** https://arxiv.org/pdf/2512.24601

## Key Takeaways

- RLMs treat long prompts as variables in a REPL environment rather than feeding them directly into the LLM's context window, enabling symbolic manipulation of arbitrarily long inputs through code execution and recursive self-invocation.
- RLM(GPT-5) achieves 91.3% on BrowseComp+ (1K documents, 6-11M tokens) at an average cost of $0.99, outperforming all baselines including summary agents (70.5%) and CodeAct+BM25 (51.0%).
- On OOLONG-Pairs (quadratic complexity), vanilla GPT-5 and Qwen3-Coder score <0.1% F1 while RLMs achieve 58.0% and 23.1% F1 respectively, demonstrating emergent capability on information-dense tasks.
- RLMs process inputs up to two orders of magnitude beyond model context windows (10M+ tokens) while maintaining strong performance, unlike base models which degrade severely with increasing context length.
- Three key design choices distinguish RLMs from existing scaffolds: (1) symbolic handle to the prompt instead of loading it into context, (2) output through REPL variables enabling unbounded generation, and (3) symbolic recursion allowing programmatic sub-LM invocation inside loops.
- RLM-Qwen3-8B, the first natively recursive language model, was trained on only 1,000 filtered trajectories from Qwen3-Coder-480B-A35B and improved base Qwen3-8B performance by a median of 28.3% across four evaluation tasks.
- Median RLM inference cost is comparable to or cheaper than base model calls, though high-variance tail costs exist due to differing trajectory lengths; RLMs are up to 3x cheaper than summary agents while achieving stronger performance.
- Different models exhibit distinct RLM behaviors: GPT-5 is conservative with sub-calls (~10 per task) while Qwen3-Coder makes hundreds to thousands, requiring prompt-level adjustments to prevent excessive recursion.

## One-Line Summary

Recursive Language Models offload long prompts into a REPL environment and enable LLMs to programmatically decompose, filter, and recursively sub-query over arbitrarily long inputs, dramatically outperforming vanilla LLMs and existing long-context scaffolds on tasks scaling up to 10M+ tokens.

## Topics

recursive language models, long-context processing, inference-time scaling, REPL environment, symbolic recursion, context management, LLM agents, sub-agent delegation
