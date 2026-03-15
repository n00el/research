# LLM Enron: experiments on structure vs scale

## Key Takeaways

- **Structure beats scale**: Giving AI agents explicit thread IDs and role identity improves performance more than adding more agents or using bigger models. Thread IDs alone significantly boost an agent's ability to handle 50-100 concurrent email threads.
- **Model intelligence still matters**: Better structure cannot compensate for insufficient model capability. GPT 5 mini with structure could not match GPT 5.2 without it -- the model must be smart enough to leverage the provided state.
- **Shared coordination state is essential**: Multi-agent setups without a shared board (0.46 quality) performed worse than a single agent with a shared board (0.63 quality). "Don't build a swarm before you build a board."
- **Role identity is distinct from task identity**: Agents can correctly identify what task to work on but still drift on who they should respond as. Making 'route_to' and 'respond_as' explicit canonical fields eliminated unauthorized response drift entirely.
- **The real bottleneck is state structure, not comprehension**: LLMs can now handle long, complex threads -- the problem is that we keep asking them to reconstruct task identity, role identity, and coordination state from conversational memory on every turn.
- **Enron email data provides realistic org complexity**: Real inboxes have 50-100 concurrent threads with limited context, requiring organizational insight. Volume predicts juggling ability; seniority does not (controlling for volume).

## One-Line Summary

Experiments using the Enron email dataset show that AI agents in organizational settings need explicit thread state, shared coordination boards, and role identity anchors -- structural scaffolding matters more than raw model scale.

## Topics

agents, multi-agent, coordination, organizational-ai, structure-vs-scale, enron, email, role-identity, shared-state, experiments, agent-architecture, swarms
