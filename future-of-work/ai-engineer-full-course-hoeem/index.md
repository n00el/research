# I Want to Become an AI Engineer (Full Course)

**Author:** hoeem (@hooeem) — "helping you turn screen time into cash flow using modern stacks"
**Date:** March 5, 2026
**Source:** https://x.com/hooeem/status/2029660075774574660
**Stats:** 1,387 likes, 171 retweets, 35 replies, 4,175 bookmarks, 401,181 views

---

A comprehensive framework for becoming an AI engineer, arguing that prompt engineering alone is insufficient. It structures AI engineering competency into three core layers plus a practical skills roadmap.

**Core thesis:** "Most of you think prompt engineering is all you need to master AI (spoiler, it's not), in fact, it's the least important vs context & intent engineering."

## Layer 1: Prompt Engineering (Foundation)

- Understanding that you are "manipulating the probabilistic distribution of the model's next-token generation"
- Key techniques: in-context learning (zero-shot, one-shot, few-shot), cognitive scaffolding (chain-of-thought forcing the model to use "temporary working memory"), and structural formatting using XML tags like `<context>`, `<task>`, `<example>`

## Layer 2: Context Engineering (Multiplier)

> "Your AI agent is only as capable as the information it can immediately access"

- **Advanced RAG architecture:** decoupling retrieval into a Search Stage (100-256 tokens) and a Retrieve Stage (1024+ tokens)
- **Context as Code:** treating context as version-controlled infrastructure with AGENTS.md files
- **MCP (Model Context Protocol):** described as a "USB-C port for AI applications" providing standardized tool/data access

## Layer 3: Intent Engineering (Strategic)

Defined as "the structural design of AI systems around goals, constraints, and measurable business outcomes."

**7-Component Intent Framework:**
1. Objective / Unlock Question
2. Desired Outcomes / Completion Criteria
3. Health Metrics / Non-Regression Guardrails
4. Strategic Context / Framework Loading
5. Constraints / Quality Standards
6. Decision Autonomy / Delegation Protocol
7. Stop Rules / Kill Switches / Circuit Breakers

Positioned as the antidote to "vibe coding," replacing informal descriptions with strict prompt contracts and intent schemas.

## Layer 4: Practical Skills Roadmap

- **Prompt engineering skills:** structural formatting, few-shot demos, chain-of-thought, meta prompting (AI improving its own prompts)
- **Context engineering skills:** context auditing, building context files, RAG pipeline design, MCP server design
- **Intent engineering skills:** outcome definition, 7-component intent schema, trade-off hierarchies, audit trail architecture

## Case Study: Klarna

Klarna's AI agent resolved support tickets in 2 minutes vs. 11 minutes for humans, saving $60 million. However, it "optimized ruthlessly for speed metrics while alienating customers," ultimately requiring the rehiring of human agents.

> "The AI lacked the capacity to balance trade-offs... it saved $60 million and became the 'public face of AI gone wrong.'"

This illustrates why intent engineering (aligning AI with business goals and constraints, not just technical performance) is critical.

## Notable Quotes

- "Building with language models is less about finding the perfect phrase and more about answering: what specific configuration of context is most likely to generate the desired behaviour?"
- "Eloquent wording cannot substitute for structural memory and systemic awareness."
- "Intent engineering is the professional antidote" to "vibe coding."

The article includes 12+ reusable prompt templates covering structural formatting, chain-of-thought analysis, RAG architecture design, MCP server blueprints, and intent schema generation.
