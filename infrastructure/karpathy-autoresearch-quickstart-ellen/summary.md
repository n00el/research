# Summary: Karpathy's autoresearch Quickstart

## One-Line Summary

Ellen introduces autoresearch-gen, a tool that wraps Karpathy's autoresearch into a single `make gen` command with LLM-scaffolded experiment setup, Streamlit dashboard tracking, and Excalidraw architecture diagrams.

## Key Takeaways

- **One-command AI research:** autoresearch-gen reduces the setup friction of Karpathy's autoresearch by using an LLM to generate `program.md`, experiment structure, and training code from a single `make gen` command with CONTEXT, DATA, and GOALS parameters.
- **Built-in experiment dashboard:** A Streamlit + Plotly dashboard (`make dashboard`) provides real-time experiment tracking, stats visualization, and architecture diagrams without needing Jupyter notebooks.
- **Autonomous overnight experiments:** The agent runs experiments in a loop while you sleep or do other things -- 30 experiments in one lunch session yielded a 41% keep rate and ~26% val_bpb improvement on TinyStories.
- **Context loss is a real problem:** After many iterations, agents can start forgetting important variables and experiment details, pointing to the need for more robust state management in long-running research loops.
- **Resilience matters for agent systems:** Karpathy's own autoresearch labs were wiped out during an OAuth outage, highlighting the need for failover and recovery mechanisms in autonomous AI research setups.
- **Lowers the barrier to entry:** The project's explicit goal is making AI research easier to start for newcomers who may not have experience structuring ML experiments from scratch.

## Topics

- autoresearch, Andrej Karpathy, autonomous AI research, LLM experiment automation, agent-driven ML, Streamlit dashboard, experiment tracking, context management, AI agent resilience, attention-free architectures, val_bpb optimization, TinyStories, Claude Code, mem0
