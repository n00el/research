# Pal: a personal context-agent that learns how you work

**Author:** Ashpreet Bedi (@ashpreetbedi)
**Date:** Mar 6, 2026
**Source:** https://x.com/ashpreetbedi/status/2029953139856531528

## Key Takeaways

- "Context agents" navigate across heterogeneous systems (SQL, files, email, calendar) rather than embedding everything into a vector store -- they query each system on its own terms instead of flattening data into similarity-searchable chunks.
- Pal maintains a "context graph" with four layers: SQL database for structured memory, markdown files for configuration/behavior, a knowledge index (map of where things live), and learnings (compass of what retrieval strategies work).
- The agent's five-step execution loop -- Classify, Recall, Retrieve, Act, Learn -- starts with intent classification to scope the retrieval before searching, making recall targeted rather than broad.
- Pal manages its own database schema and evolves it as data grows, leveraging the fact that language models are remarkably good at SQL.
- Scheduled background tasks (daily briefings, inbox digests, weekly reviews) are what turn Pal from a reactive tool into a compounding system that has context ready before you ask.
- Governance is enforced at the "external impact" boundary: the agent can read freely but gates any action that affects other people (email sends become drafts, calendar invites with external attendees require confirmation).
- The key architectural insight is that the prompt stays the same but the agent improves over time because its structured memory, navigation map, and learned strategies all grow with use.

## One-Line Summary

Pal is an open-source personal agent built on Agno that replaces RAG-style vector search with a "context graph" approach -- navigating across SQL, files, email, and calendar while learning which retrieval strategies work best for each user over time.

## Topics

AI agents, context agents, personal assistant, RAG alternatives, Agno, open source, developer tools, knowledge management
