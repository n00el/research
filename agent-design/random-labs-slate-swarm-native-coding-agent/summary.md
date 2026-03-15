# We built RLM for coding -- Slate: The First Swarm-Native Coding Agent

**Author:** akira (@realmcore_) / Random Labs
**Date:** March 12, 2026
**Source:** https://x.com/realmcore_/status/2032146316730778004

## Key Takeaways

- Slate is the first frontier coding agent built around swarm orchestration, using a novel "Thread Weaving" architecture that dispatches parallel worker threads from a central orchestrator via a TypeScript DSL.
- Unlike traditional multi-agent systems that use message passing, Slate shares context directly between the orchestrator and worker threads, functioning as a "hive mind" rather than isolated subagents.
- Slate solves the "knowledge overhang" problem -- where models have latent intelligence they cannot access when tactically overwhelmed -- by decomposing work into bounded parallel tasks.
- Multi-model orchestration is a first-class feature: developers can have Claude Sonnet planning, GPT-5.4 executing code, and GLM 5 researching docs simultaneously in a single session.
- Early benchmarks show Slate passing 2/3 of make-mips-interpreter tests on Terminal Bench 2.0, a task where frontier models in standard harnesses succeed less than 20% of the time.
- The underlying technique, Recursive Language Models (RLM), uses REPL reference semantics to let agents think about execution graphs rather than individual operations.

## One-Line Summary

Random Labs' Slate introduces "Thread Weaving" -- a swarm-native architecture where a central orchestrator programs in action space via TypeScript DSL, dispatching parallel subagents across multiple models to solve complex coding tasks that defeat single-agent approaches.

## Topics

swarm orchestration, multi-agent systems, coding agents, recursive language models, thread weaving, AI developer tools
