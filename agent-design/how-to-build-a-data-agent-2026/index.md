# How to Build a Data Agent in 2026

**Author:** Jamie Quint (@jamiequint)
**Date:** 2026-03-05
**Source:** https://x.com/jamiequint/status/2029705203457609785
**Stats:** 19 replies, 66 retweets, 798 likes, 2,377 bookmarks, 292,060 views

---

![Cover image](image-1.jpg)

I built the initial data stack at Notion in 2020 when the "Modern Data Stack" was first becoming a thing, and have spent some time over the last year consulting or helping friends' companies solve the next revolution, or the "AI Data Stack". Here is a how-to guide for building a modern AI data agent that can 5x+ your data team bandwidth. This particular build took me about 3 weeks end-to-end, including the third party integrations (Slack/Notion) and the internal admin dashboard to manage it.

## From the Modern Data Stack to the AI Data Stack

The "modern data stack" was about making your data accessible (Fivetran/Airbyte/etc), organized (DBT), queryable (Snowflake/Bigquery), and available anywhere (Census/Hightouch). The modern data stack made data accessible to analysts. The AI data stack makes data accessible to everyone, by replacing the human translation layer (write SQL, build dashboard, schedule report, interpret results) with agents that go from question to answer automatically.

Advanced companies were doing automated reporting with AI starting in late-2024/early-2025 with models like GPT-4o layered on top of DBT MetricFlow or Cube.dev, but it took a lot of handholding and careful semantic-layer management. With the release of Opus 4.6, and specifically Opus 4.6, even though Opus 4.6 loses pitifully on coding tasks vs Codex 5.3 xhigh, it crushes Codex 5.3 xhigh in side-by-side testing for data analysis, the need for an advanced semantic layer has completely collapsed (which is great, because it was a pain in the ass to begin with), and can now be entirely replaced with context management.

## Killing the Semantic Layer with Context Management

The semantic layer is a static, hand-maintained mapping between business concepts and SQL (metrics, dimensions, relationships). It is a pain in the ass to build and a pain in the ass to maintain. Context management is the inverse: instead of pre-defining everything the agent might need, you give it access to the raw source of truth (your DBT models, your app code) and let it investigate on demand, per-question.

Before writing any SQL, you just dispatch a sub-agent (I call it a context agent) to read the actual transformation logic in the dbt models and trace ref() dependencies upstream through the DAG. If you annotate your DBT files with metadata links into application code (highly recommended, and easy to automate) it will also validate business rules. It builds a brief for itself: relevant tables, column definitions, join paths, filters, dedup logic, caveats. This is the work a semantic layer was supposed to pre-compute, but now it happens dynamically at query time.

The other great part about context management compared to the semantic layer is that it's adaptive. When a user corrects the agent ("no, that metric needs this filter" or "that table has duplicate rows, you need to dedup on X"), you can extract those corrections into durable, reusable knowledge that gets retrieved on future questions. This is the semantic layer rebuilding itself from usage, rather than requiring an analyst to maintain it.

Semantic layers were brittle, they required constant maintenance, they couldn't cover edge cases, and they created a bottleneck where the data team had to anticipate every question. Context management scales with the codebase (because it reads the codebase) and scales with usage (because it learns from corrections and input from the team).

## Basic Set Up

1. **Set up an agentic loop on your host machine.** I recommend the Claude Agent SDK since Opus 4.6 > Codex 5.3 xhigh when it comes to data.

2. **Ensure your dbt repo and codebase or codebases are accessible from the agentic loop.**

3. **Annotate your dbt base models** (usually stg_) with links to application code and a high-level description of their functionality. This can mostly be done automatically.

4. **Build out a context sub-agent that runs before SQL generation.** When a question comes in, before your main agent writes any SQL, dispatch a sub-agent whose only job is to investigate the data landscape. It reads the relevant model files (the full transformation logic, not just headers), traces upstream dependencies, checks application code if annotations exist, and returns a structured brief: tables, columns, join paths, filters, dedup rules, caveats. This brief gets injected into the main agent's context. The sub-agent is cheap (it's mostly reading files) and it's what keeps the main agent from hallucinating table structures.

5. **Build a knowledge store for learned corrections, or "quirks."** When a user corrects the agent, extract the correction into a durable "quirk," a short, reusable piece of knowledge like "the orders table has duplicate rows per order when there are multiple shipments; always dedup on order_id." Store these with embeddings for semantic search. On each new question, run hybrid retrieval (vector similarity + keyword search) against your quirk store and inject the top matches alongside the context agent's brief. Over time this becomes your institutional knowledge base, the stuff that used to live in one analyst's head. I use Postgres with pgvector for vector similarity, OpenAI text-embedding-3-small for embeddings, and pg_textsearch for BM25 keyword search. It works fine.

6. **Add human-authored metric definitions for your core KPIs,** the metrics that get asked about constantly and have precise definitions. Let your data team author structured definitions with inference guidance (how to calculate it, what filters apply, etc). These go into the same knowledge store and get retrieved the same way as quirks. Think of this as the 20% of the semantic layer that was actually useful, maintained by humans when they feel like it rather than required for the system to function.

7. **Connect this all to Slack** (ensuring you set up multi-threading for the agents) and build a basic admin UI dashboard to monitor ongoing usage/results and enable editing of the knowledge store.

## Advanced Tuning

Once the basic loop is working (context agent investigates, main agent writes SQL, answers come back) you're going to notice that sometimes the SQL is wrong in ways the agent doesn't catch. It'll run a query that executes fine but answers a slightly different question than what was asked, or it'll miss a filter, or it'll join on the wrong key and silently double-count. The agent doesn't know it got it wrong, so it confidently delivers a bad answer. A naive way to improve this would be to brute force adding more metrics definitions and quirks, or to add more documentation to your DBT files, but there's an elegant solution that's actually much simpler.

The fix is to make the agent score its own work, here is how this works:

1. It scores every SQL query on three dimensions: structural correctness, execution reliability, and context alignment. This uses deterministic signals plus Haiku for semantic/context checks (including subquestion coverage), with deterministic fallback if Haiku is unavailable.

2. It then builds a context-gap brief from those scored queries and their evidence. The brief breaks the question into subquestions, marks what is fully supported by high-confidence evidence, and flags missing dimensions, entities, time windows, and business logic.

3. That brief drives the recovery loop. If unresolved gaps remain and there is enough SQL signal, the agent retries with a targeted prompt focused on specific gaps; if the issue looks like schema/context mismatch, it reruns context enrichment first.

For retrieval tuning, your hybrid search needs a few knobs. Collection weights let you balance how much to favor metrics vs. quirks in results. A reviewed-item multiplier ensures human-approved knowledge ranks higher than stuff the agent learned on its own (which may or may not be right). Use reciprocal-rank fusion to blend your vector and BM25 candidates into a single ranked list. Set a fixed context budget so you're not stuffing the entire knowledge store into every prompt, which can also be performance destroying.

## Feel the AGI

This setup reduced what we thought was going to be an internal hiring plan for 4-5 data analysts at a friend's company to only hiring one. Customer Success and Sales can self-serves data in Slack now even for complex questions. Product/Growth can self-serve data in Slack now and instantly save it to Notion. Welcome to the future.
