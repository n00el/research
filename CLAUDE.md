# Research Knowledge Base

This folder contains structured research notes on AI agents, agentic coding, Claude Code, MCP, and related topics.

## QMD Search

A QMD index is configured for this folder. Use the MCP tools to search across all research:

- **`qmd_deep_search`** — Best quality. Hybrid search with query expansion + re-ranking. Use for open-ended questions like "how should agents handle long tasks?"
- **`qmd_search`** — Fast BM25 keyword search. Use when you know the exact terms.
- **`qmd_vector_search`** — Semantic similarity. Use when the question may not match exact wording in docs.
- **`qmd_get`** — Fetch a specific document by path or docid.
- **`qmd_multi_get`** — Batch fetch via glob or comma-separated list.

Always prefer QMD search over grep/glob when looking for information across research topics. It understands semantic meaning, not just keywords.

## Maintenance

When new research is added:

```sh
qmd update        # Re-index new/changed files
qmd embed         # Generate embeddings for new docs
```

## Structure

Research is organized into 9 thematic categories:

```
research/<category>/<topic>/
├── index.md      — Full research content
└── summary.md    — Condensed summary (where available)
```

### Categories

| Category | Description |
|---|---|
| `agent-design` | Agent architecture, frameworks, multi-agent, orchestration, engineering |
| `claude-code` | Claude Code / Cowork features, setup, best practices, prompting |
| `skills-and-knowledge` | Skills creation, skill graphs, CLAUDE.md, self-improving skills |
| `context-engineering` | Context mgmt, memory, prompt caching, knowledge retrieval, file-system patterns |
| `harness-engineering` | Harness/scaffold design, cybernetics, evaluation, spec-driven dev |
| `infrastructure` | Production deployment, sandboxes, cloud, long-running agents |
| `tools-and-integrations` | MCP, tool calling, browser tools, API, benchmarks |
| `industry` | Product releases, company analysis, market trends, code quality |
| `future-of-work` | Career, labor market, AI disruption, decision theory, vibe coding |

Root-level files (`CLAUDE.md`, `DIGEST.md`, `KNOWLEDGE-BASE.md`, `READING-PRIORITY.md`, `SUMMARY.md`) remain at `research/`.
