# How to deploy autoresearch

**Author:** hoeem (@hooeem)
**Date:** March 8, 2026
**Source:** https://x.com/hooeem/status/2030720614752039185

## Key Takeaways

- Karpathy's autoresearch lets an AI agent autonomously run ~100 LLM training experiments overnight on a single GPU, keeping improvements and discarding failures based on the val_bpb metric
- Mac users need the miolini/autoresearch-macos fork (Apple Silicon M1-M4), while Windows/Linux users need an NVIDIA GPU and the original karpathy/autoresearch repo
- The setup requires only three free tools (Terminal, Git, uv) plus an AI agent (Claude Code at $20/mo for full autopilot, or Cursor for free semi-manual mode)
- The human's only job is writing and refining program.md -- the meta-prompt that instructs the AI agent on what experiments to try
- The Mac fork is safe: Karpathy links to it himself, the developer (Artem Andreenko) has 167 public GitHub projects, and the entire codebase is ~630 lines of readable Python
- Out of ~100 overnight experiments, expect only 10-20 to be actual improvements -- this is normal research behavior, and the agent handles the filtering automatically
- The article includes a condensed quick-start guide that covers the entire setup process in about 30 minutes

## One-Line Summary

A comprehensive beginner-friendly walkthrough for deploying Karpathy's autoresearch on any platform (Mac/Windows/Linux), turning your computer into an autonomous AI research lab that runs experiments while you sleep.

## Topics

autoresearch, karpathy, AI agents, autonomous research, LLM training, Claude Code, GPU computing, developer tools
