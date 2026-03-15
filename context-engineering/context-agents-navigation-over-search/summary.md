# Context Agents: Navigation over Search

**Author:** Ashpreet Bedi (@ashpreetbedi)
**Date:** 2026-03-10
**Source:** https://x.com/ashpreetbedi/status/2031416367610744960

## Key Takeaways

- Context engineering has evolved through three generations: Semantic RAG (2023), Agentic RAG (2024-25), and Agentic Navigation (2026+), each solving limitations of the previous.
- The core insight of "context agents" is that data sources should be queried via their native interfaces (SQL for databases, time ranges for calendars, sender/date for email) rather than flattening everything into vector embeddings.
- A context agent operates through a classify-recall-read-act-learn loop, where each interaction improves the next via accumulated knowledge (structural map) and learnings (retrieval strategies).
- Knowledge (shared, structural: where things live) and Learnings (per-user, behavioral: how to retrieve them) are distinct systems that together enable progressive improvement.
- Proactive scheduled tasks (daily briefings, inbox digests, weekly reviews) transform a reactive assistant into a persistent context-aware partner.
- The open-source "Pal" project demonstrates this pattern using a local file system, Gmail, Google Calendar, Slack, and PostgreSQL as heterogeneous context sources.

## One-Line Summary

Agents should navigate heterogeneous data sources through their native interfaces and learn retrieval strategies over time, rather than flattening everything into vector search.

## Topics

context engineering, AI agents, RAG, information retrieval, personal AI assistant, agentic systems
