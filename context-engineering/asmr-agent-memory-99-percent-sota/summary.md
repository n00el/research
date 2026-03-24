# We Broke the Frontier in Agent Memory: ~99% SOTA on LongMemEval

**Author:** Dhravya Shah (@DhravyaShah)
**Date:** Mar 22, 2026
**Source:** https://x.com/DhravyaShah/status/2035517012647272689

## Key Takeaways

- Supermemory's ASMR (Agentic Search and Memory Retrieval) technique achieved ~99% accuracy on LongMemEval by replacing vector search with active agentic reasoning -- no embeddings or vector DB required
- The architecture uses 3 parallel observer agents for ingestion and 3 parallel search agents for retrieval, each with specialized focus areas (facts, context/implications, temporal timelines)
- Two answering approaches were tested: an 8-variant ensemble (98.60%) where any correct path counts, and a 12-variant Decision Forest with aggregator LLM (97.20%) for single authoritative answers
- The key insight is that agentic retrieval eliminates the "semantic similarity trap" where vector search cannot distinguish stale facts from updated corrections
- The author later revealed this was partially a "stunt" to prove a point about benchmarking, though the technical results are real
- Code will be open-sourced in early April 2026 via github.com/supermemoryai

## One-Line Summary

Supermemory achieved ~99% on LongMemEval by replacing vector search with parallel specialized agents that actively reason over stored knowledge, proving that agentic retrieval fundamentally outperforms embedding-based RAG for long-term memory.

## Topics

agent memory, RAG alternatives, agentic retrieval, benchmarks, multi-agent systems, context engineering
