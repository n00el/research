# CLAUDE.md Masterclass: From Start to Pro-Level User with Hooks & Subagents

**Author:** Joe Njenga
**Date:** January 28, 2026
**Source:** https://newsletter.claudecodemasterclass.com/p/claudemd-masterclass-from-start-to

## Key Takeaways

- CLAUDE.md is a configuration file injected into Claude's system prompt, not documentation -- Claude follows its instructions more strictly than anything typed in chat.
- Frontier LLMs reliably follow roughly 150-200 instructions total; since Claude Code's system prompt already uses ~50, you have about 100-150 slots before instruction-following degrades uniformly across all rules.
- Use progressive disclosure: keep CLAUDE.md lean with only universally applicable rules, and offload task-specific instructions to separate files referenced by path.
- The three-level hierarchy (global, project root, nested directory) lets you layer shared conventions with context-specific rules that load only when Claude touches those directories.
- Do not use CLAUDE.md as a linter; enforce code style through actual tooling (ESLint, Black, Ruff) and pre-commit hooks instead of burning instruction budget on formatting rules.
- CLAUDE.md compounds in value when paired with hooks (automated enforcement) and subagents (isolated task execution with clean context), forming a rules-define / hooks-enforce / subagents-execute system.

## One-Line Summary

CLAUDE.md is a system-prompt-level configuration file with a hard instruction budget, so keeping it lean, hierarchical, and paired with hooks and subagents is the key to reliable Claude Code automation.

## Topics

claude code, claude.md, prompt engineering, developer tooling, ai-assisted development, agentic workflows
