# How to Build a Data Agent in 2026

**Author:** Jamie Quint (@jamiequint)
**Date:** 2026-03-05
**Source:** https://x.com/jamiequint/status/2029705203457609785

## Key Takeaways

- The "AI Data Stack" replaces the human translation layer (SQL, dashboards, reports) with agents that go directly from question to answer, making data accessible to everyone rather than just analysts.
- Semantic layers are dead -- instead of pre-defining business concepts in a static mapping, use a "context agent" that dynamically reads DBT models, traces dependencies, and builds per-query briefs at runtime.
- User corrections should be extracted into durable "quirks" (stored with embeddings for hybrid retrieval), creating a self-building institutional knowledge base that replaces the analyst who "just knows" things.
- The architecture is: context sub-agent (reads DBT/code) -> knowledge retrieval (quirks + metrics via pgvector + BM25) -> main agent (writes SQL) -> self-scoring loop (structural, execution, context alignment checks) -> recovery if gaps found.
- For advanced tuning, make the agent score its own SQL on three dimensions and build context-gap briefs that drive targeted recovery loops rather than brute-forcing more documentation.
- This setup reduced a planned 4-5 analyst hiring plan to just one hire, with CS/Sales/Product self-serving data through Slack.

## One-Line Summary

A practical architecture for replacing semantic layers with dynamic context agents and learned "quirks," enabling non-technical teams to self-serve complex data questions through Slack.

## Topics

AI agents, data engineering, modern data stack, semantic layer, dbt, Claude Opus
