# Agentic Software Engineering

**Author:** Ashpreet Bedi (@ashpreetbedi) — Founder of Agno (formerly Phidata)
**Date:** March 6, 2026
**Source:** https://x.com/ashpreetbedi/status/2029953139856531528

---

Building agents is easy; running them in production is hard. The moment an agent needs to handle identity, state, concurrency, sensitive actions, and failure recovery, it becomes a distributed system.

## Three-Part Framework: Build. Serve. Connect.

### Build
Define the model, tools, knowledge, memory, storage, guardrails.

### Serve
Deploy as a horizontally scalable API with persistent storage, streaming, background execution, and retry logic. This is where most products stall.

### Connect
Integrate agents where users actually are -- products, Slack, Discord, MCP.

## Six Pillars of Agentic Software

1. **Durability** -- Handle multi-step reasoning failures through checkpointing and resumption.

2. **Isolation** -- Enforce user boundaries across every resource (DB, vector store, model provider).

3. **Governance** -- Layered authority: auto-execute, user-approval, and admin-approval tools.

4. **Persistence** -- Store sessions, memory, and knowledge for cross-conversation learning.

5. **Scale** -- Manage external dependencies (model APIs, third-party tools) with rate limits and latency you do not control.

6. **Composability** -- Structure agents as standard API services callable by other agents, frontends, and bots.

## Key Takeaway

Agent engineering is 40% agent development, 40% system design, and 20% security engineering. Teams treating agents as scripts will miss the mark; those internalizing these six pillars early will ship superior products.

## About the Author

Ashpreet Bedi is the founder of Agno (formerly Phidata), a multi-agent framework, runtime, and UI. He previously worked at Airbnb and Facebook.

*Note: The original X Article required JavaScript to render. Content inferred from the author's published blog post "Agentic Software Engineering" (March 1, 2026) which matches this tweet's publishing timeline.*
