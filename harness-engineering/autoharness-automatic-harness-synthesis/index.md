# AutoHarness: Improving LLM Agents by Automatically Synthesizing a Code Harness

**Author:** DAIR.AI (@dair_ai), referencing paper by Xinghua Lou, Miguel Lazaro-Gredilla, Antoine Dedieu, Carter Wendelken, Wolfgang Lehrach, Kevin P. Murphy
**Date:** March 7, 2026 (tweet); paper submitted February 10, 2026
**Source:** https://x.com/dair_ai/status/2030289894808166866
**Stats:** 462 likes, 51 retweets, 15 replies, 869 bookmarks, ~99,300 views
**Paper:** https://arxiv.org/abs/2603.03329

---

## What is an Agent Harness?

The scaffolding that lets an agent interact with its environment -- tools, code execution, file systems, and APIs. It is the structural layer between the LLM and the outside world.

## Core Idea

Rather than manually engineering these harnesses, the paper demonstrates that they can be **automatically synthesized** by LLMs themselves (specifically Gemini-2.5-Flash) through iterative refinement.

## Problem Addressed

LLM agents frequently attempt actions that are not just suboptimal but are strictly prohibited by their environment. AutoHarness creates protective code structures that prevent these illegal actions.

## Results — Smaller Models Beating Larger Ones

- Gemini-2.5-Flash with AutoHarness-generated safeguards **outperformed larger models** like Gemini-2.5-Pro across 145 TextArena games.
- When extended to full policy code generation, the approach beat both Gemini-2.5-Pro and GPT-5.2-High on 16 single-player games while **reducing costs**.

## Implications

- **Cost efficiency:** A smaller, cheaper model (Flash-tier) can surpass premium-tier models when equipped with the right scaffolding.
- **Self-improvement loop:** The LLM itself generates the code harness iteratively, meaning the system improves its own operational constraints without human intervention.
- **Practical significance:** The bottleneck for agent performance may not be model size but rather the quality of the harness/scaffolding wrapping the model.

> Automatically synthesizing harnesses could "dramatically lower the barrier to building effective agents."
