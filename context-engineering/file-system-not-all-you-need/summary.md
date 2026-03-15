# a file system is not all you need

**Author:** Kevin Gu (@gukevinr)
**Date:** March 12, 2026
**Source:** https://x.com/gukevinr/status/2031889622385729730

## Key Takeaways

- Markdown-based knowledge graphs are effectively reinventing databases in a worse substrate -- they lack joins, indexes, constraints, and dependency tracking that real databases provide.
- The core maintenance problem is structural duplication: extracting claims from source docs into markdown notes creates two copies that inevitably drift, and no amount of agent diligence fixes this.
- The code analogy for file-based context fails because organizational knowledge lacks executability -- there are no unit tests to catch when a decision contradicts the roadmap or when information becomes stale.
- Markdown flattens epistemological distinctions: decisions, guesses, stale beliefs, and personal interpretations all look structurally identical as text, making governance impossible.
- Schema itself is context -- typed data tells agents not just what something says, but its provenance, dependencies, authority status, and what needs re-evaluation if it changes.
- The proposed architecture is three layers: work apps (source of truth), file layer (agent instructions and SOPs), and database layer (structured relationships with provenance and dependency tracking).
- The end state is agents that write their own ETL pipelines, maintaining a live structured graph that cascades updates when sources change and learns from rejected suggestions.

## One-Line Summary

Markdown knowledge graphs are a poor man's database -- organizations need typed schemas with provenance tracking and dependency chains, not prettier wikis, for agents to graduate from copilots to coworkers.

## Topics

context engineering, knowledge management, agent architecture, structured data, organizational knowledge, databases vs filesystems
