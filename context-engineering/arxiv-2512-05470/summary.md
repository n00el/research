# Everything is Context: Agentic File System Abstraction for Context Engineering

**Authors:** Xiwei Xu, Xuewu Gu, Robert Mao, Yechao Li, Quan Bai, Liming Zhu
**Date:** 5 December 2025
**Source:** https://arxiv.org/pdf/2512.05470

## Key Takeaways

- Proposes a Unix-inspired "everything is a file" abstraction for context engineering, where heterogeneous context sources (memory, tools, knowledge, human input) are mounted and accessed through a uniform virtual file system (VFS) interface.
- Identifies three core design constraints imposed by GenAI models that shape the context pipeline: bounded token windows, inherent statelessness, and non-deterministic probabilistic outputs.
- Introduces a three-component Context Engineering Pipeline consisting of Context Constructor (selection and compression), Context Updater (streaming and refresh), and Context Evaluator (validation and memory write-back with human-in-the-loop).
- Defines a Persistent Context Repository with three layers -- History (immutable raw logs), Memory (structured indexed views with multiple types), and Scratchpad (ephemeral task workspaces) -- each with distinct persistence and governance policies.
- Presents a taxonomy of seven memory types (scratchpad, episodic, fact, experiential, procedural, user, historical record) classified along temporal, structural, and representational dimensions.
- Implemented within the open-source AIGNE framework, where the AFS module provides list/read/write/search operations with sandboxed access, ripgrep integration, and programmable resolvers for mounting external services.
- Demonstrates practical applicability through two exemplars: a memory-enabled agent using SQLite-backed persistent memory, and a GitHub MCP server mounted as a file system module for uniform tool access.
- Aligns with the LLM-as-Operating-System paradigm but advances it from a metaphor to a concrete software architecture grounded in SE principles (abstraction, modularity, encapsulation, separation of concerns, traceability).

## One-Line Summary

A file-system abstraction inspired by Unix's "everything is a file" philosophy that provides a persistent, governed infrastructure for context engineering in GenAI systems, turning ad-hoc prompt and memory management into a traceable, verifiable architectural discipline.

## Topics

context engineering, file system abstraction, LLM-as-OS, agentic AI, memory management, MCP, software architecture, human-AI co-work
