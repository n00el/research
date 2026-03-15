# Just Talk To It - the no-bs Way of Agentic Engineering

**Author:** Peter Steinberger
**Date:** October 14, 2025
**Source:** https://steipete.me/posts/just-talk-to-it

## Key Takeaways

- GPT-5-Codex at mid settings with 3-8 parallel terminal instances is the author's optimal setup for solo agentic engineering on a ~300k LOC TypeScript codebase, beating worktrees and PR-based workflows.
- "Blast radius" thinking -- estimating how many files a change touches and how long it takes -- is the core planning heuristic; stop agents mid-execution and reassess when things take longer than expected.
- Codex significantly outperforms Claude Code in token efficiency (~230k vs 156k usable context), speed (Rust rewrite), message queuing, and communication style, which the author says materially affects his mental health.
- Third-party harnesses (Amp, Factory, Cursor agents) lack durable competitive advantage since first-party tools converge on the same features; $1k/month in subscriptions beats API pricing by ~10x.
- MCPs are mostly marketing theater -- CLIs are superior because they carry zero context tax and models already know how to use them (GitHub MCP = 23k tokens vs `gh` CLI = 0 tokens).
- Dedicated refactoring time (~20% of work) using tools like jscpd, knip, and eslint plugins keeps agent-generated code quality high without manual effort.
- Subagents, plugins, RAG, and elaborate plan modes are workarounds for weaker models; with GPT-5-Codex, short conversational prompts (often 1-2 sentences + a screenshot) are sufficient.

## One-Line Summary

Peter Steinberger argues that the best agentic engineering workflow is radically simple -- run multiple Codex CLI instances in parallel, write short prompts, and skip the elaborate tooling ecosystem that mostly patches model weaknesses.

## Topics

agentic engineering, developer workflow, codex cli, claude code, ai coding tools, prompt engineering, code quality
