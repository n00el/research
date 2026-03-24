# code-review-graph: Local Knowledge Graph for Claude Code

**Author:** tirth8205
**Date:** 2026
**Source:** https://github.com/tirth8205/code-review-graph?tab=readme-ov-file

## Key Takeaways

- Builds a Tree-sitter AST graph of your codebase so Claude reads only the files affected by a change, achieving 6.8x fewer tokens on reviews and up to 49x on daily coding tasks
- Works as a Claude Code plugin or pip package with automatic graph updates on file edits and git commits
- Supports 14 languages including Python, TypeScript, JavaScript, Go, Rust, Java, C#, Ruby, Kotlin, Swift, PHP, Solidity, C/C++
- Uses blast-radius analysis to compute the minimal set of affected functions, classes, and files for any change
- All data stored locally in SQLite -- no external database or cloud dependency required
- Includes optional semantic search via sentence-transformers and interactive D3.js visualization

## One-Line Summary

A Tree-sitter-powered local knowledge graph plugin for Claude Code that reduces token consumption by up to 49x by giving Claude precise, change-aware context instead of re-reading the entire codebase.

## Topics

claude code, code review, tree-sitter, token optimization, knowledge graph, developer tools
