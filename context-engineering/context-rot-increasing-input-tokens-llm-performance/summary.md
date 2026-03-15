# Context Rot: How Increasing Input Tokens Impacts LLM Performance

**Author:** Kelly Hong, Anton Troynikov, Jeff Huber - Chroma
**Date:** July 14, 2025
**Source:** https://research.trychroma.com/context-rot

## Key Takeaways

- LLM performance degrades consistently as input length increases, even on trivially simple tasks like word replication and basic retrieval -- models do not process their context window uniformly.
- Lower semantic similarity between the needle and question accelerates performance degradation at longer context lengths, meaning real-world tasks (which rarely have exact lexical matches) suffer more than benchmarks suggest.
- Even a single distractor meaningfully reduces model accuracy, and the effect compounds with multiple distractors -- Claude models tend to abstain under ambiguity while GPT models hallucinate confidently.
- Counterintuitively, logically coherent haystacks hurt model performance -- shuffling sentences and destroying narrative flow consistently improved retrieval across all 18 models tested.
- The type and structure of irrelevant context matters: needle-haystack semantic similarity, haystack coherence, and distractor placement all independently affect results, making "context engineering" (how information is presented) as important as what information is included.
- On the Repeated Words task, models exhibit bizarre failure modes at scale: Opus 4 refuses citing "copyright risk," Gemini produces gibberish strings, and Qwen3-8B generates stream-of-consciousness text about needing a break.

## One-Line Summary

LLMs fail increasingly and non-uniformly as context grows, even on trivial tasks, making how you structure and present information in the context window (context engineering) more important than simply fitting it all in.

## Topics

long context, context engineering, LLM evaluation, needle in a haystack, retrieval, attention mechanisms, model benchmarks
