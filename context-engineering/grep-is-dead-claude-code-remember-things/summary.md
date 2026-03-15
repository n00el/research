# Grep Is Dead: How I Made Claude Code Actually Remember Things

**Author:** Artem Zhutov (@ArtemXTech)
**Date:** March 2, 2026
**Source:** https://x.com/ArtemXTech/status/2028330693659332615

## Key Takeaways

- Claude Code sessions start from zero every time, and context compaction at 60% loses half your decisions -- the memory problem is structural, not a minor annoyance
- QMD (by Tobias Lutke / Shopify) is a local search engine that indexes Obsidian vaults with BM25, semantic, and hybrid search -- replacing grep's brute-force string matching with relevance-ranked results
- BM25 handles 80% of searches (fast, deterministic, scores by term frequency and rarity); semantic search fills the gap for fuzzy/conceptual queries where exact keywords don't exist in the vault
- The /recall skill wraps QMD with three modes: temporal (reconstruct sessions by date), topic (BM25 across collections), and graph (interactive visual exploration of session-file relationships)
- A post-session hook automatically exports and embeds Claude Code JSONL conversations into QMD, keeping the index always fresh with no manual steps
- Semantic search over personal notes can surface forgotten insights and unacted-on ideas -- the "find ideas I never acted on" query revealed forgotten projects from months prior
- The whole stack is portable: Obsidian Sync + QMD + OpenClaw means the same vault, index, and skills are accessible from any device

## One-Line Summary

Build persistent memory for Claude Code by indexing your Obsidian vault and session history with QMD's BM25/semantic search, then use a /recall skill to load full context before every conversation.

## Topics

claude code, obsidian, qmd, search, memory, context management, skills
