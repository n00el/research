# Summary: The Best Article on Actual Agent Engineering

## Key Takeaways

- **A single `run(command="...")` tool outperforms a catalog of typed function calls** — after 2 years building agents at Manus and on open-source projects, the author abandoned function calling entirely in favor of Unix-style commands.
- **Unix and LLMs share the same fundamental paradigm:** Unix's "everything is a text stream" and LLMs' "everything is tokens" converge on an identical interface model, making CLI tools a natural fit for agent tool use.
- **LLMs are essentially terminal operators** — they are faster than any human and have already seen vast amounts of shell commands and CLI patterns in their training data.
- **Two-layer architecture is recommended:** Layer 1 (Unix Execution Layer) handles pure Unix semantics like command routing, pipes, chains, and exit codes; Layer 2 (LLM Presentation Layer) handles LLM-specific constraints like binary guards, truncation, and overflow.
- **Don't reinvent the tool interface** — leverage 50 years of Unix tooling rather than building custom function-calling schemas.

## One-Line Summary

A former Manus backend lead argues that a single Unix-style `run(command="...")` tool dramatically outperforms typed function calling for AI agents, because Unix's text-stream philosophy is a natural fit for how LLMs process and produce information.

## Topics

agent-engineering, function-calling, unix-philosophy, tool-use, manus, llm-agents, cli-patterns, agent-architecture, pinix, agent-clip
