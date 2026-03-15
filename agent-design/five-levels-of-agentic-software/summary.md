# The 5 Levels of Agentic Software

**Author:** Ashpreet Bedi (@ashpreetbedi)
**Date:** Feb 20, 2026
**Source:** https://x.com/ashpreetbedi/status/2024885969250394191

## Key Takeaways

- Agents should be built incrementally across five levels: tools, storage/knowledge, memory/learning, multi-agent teams, and production systems — each level adds complexity that must be justified by a clear failure of the simpler approach.
- Tools are what differentiate an agent from a raw LLM; without the ability to read files, write files, and execute commands, an LLM cannot act on the world.
- Knowledge (agentic RAG over specs, decision records, meeting notes) fills the gap between what the LLM was trained on and what your team actually needs it to follow — much valuable context lives outside the codebase.
- The jump from Level 2 (static knowledge) to Level 3 (learning from experience) is the most impactful: agents that store learnings and build user profiles across sessions compound in quality over time.
- Multi-agent teams are powerful but unpredictable — prefer explicit workflows over dynamic delegation for production reliability, and reserve teams for human-supervised settings like code review.
- Most teams fail by jumping straight to multi-agent orchestration (Level 4) when a single well-instructed agent with good tools (Level 1-2) would have solved the problem with far less debugging.
- For production deployment, swap development-grade storage (SQLite, ChromaDb) for production databases (PostgreSQL, PgVector) and add tracing for observability into every tool call and delegation decision.

## One-Line Summary

Build agentic software by starting with the simplest viable agent and adding capabilities (knowledge, memory, multi-agent coordination, production infrastructure) only when the current level demonstrably fails.

## Topics

agentic architecture, ai agents, rag, multi-agent systems, llm tooling, incremental complexity
