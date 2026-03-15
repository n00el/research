# Summary: Why 80% of Agentic AI Work Isn't AI

## Key Takeaways

- **The agent is the easy part** -- a weekend demo becomes a quarter-long platform build because production agents need five layers of non-AI infrastructure: data engineering, state management, retry/recovery, cost governance, and observability.
- **Data engineering is Layer 1** -- real-world inputs arrive in dozens of formats (PDFs, HL7, Word docs, XML) and require normalization, chunking, deduplication, recency handling, and PII scrubbing before any LLM call.
- **State management solves the amnesia problem** -- LLMs are stateless, so multi-hour agent tasks need working memory, episodic memory, checkpointing, and external memory stores to survive context window resets.
- **Silent failures are more dangerous than loud ones** -- agents that treat API timeouts as implicit success (e.g., a travel booking agent confirming unbooked flights) cause real-world harm; production systems need idempotency keys, partial completion detection, and dead letter handling.
- **Cost governance kills more deployments than technical issues** -- a $0.12/task demo can become $4.80/task in production; teams need per-task cost attribution, model routing (cheap models for simple sub-tasks), budget circuit breakers, and anomaly alerts.
- **Observability is non-negotiable** -- trace IDs across every hop, input/output capture at each reasoning step, decision attribution, and human review audit trails are required for debugging, regulatory compliance, and trust.
- **Winning teams build platforms, not just agents** -- the 80% of non-AI work (data pipelines, state stores, retry logic, cost controls, tracing) requires the same discipline as building payment systems or distributed databases.

## One-Line Summary

Production AI agents require five layers of unglamorous but load-bearing infrastructure -- data engineering, state management, retry logic, cost governance, and observability -- that constitute 80% of the work and determine whether a weekend demo becomes a reliable system.

## Topics

agentic-ai, production-infrastructure, data-engineering, state-management, retry-logic, cost-governance, observability, agent-platform, llm-operations, engineering-leadership
