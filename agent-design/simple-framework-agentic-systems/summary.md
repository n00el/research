# A Simple Framework to Build Agentic Systems That Just Works

**Author:** AVB (@neural_avb)
**Date:** February 28, 2026
**Source:** https://x.com/neural_avb/status/2027721962479288566

## Key Takeaways

- The single unifying principle for designing agentic systems is to "put yourself in the agent's shoes" -- reason about what context, tools, and outputs you would need as a human doing the same task, then give exactly that to the agent.
- System prompts should explicitly define observations (what the agent sees), expected outcomes (success criteria), and available tools -- not just a role description.
- Lazy loading context beats pre-loading: agents perform better when they pull information on demand from an index rather than being frontloaded with everything.
- Minimize the action space -- too many tools causes "agentic dementia" (decision paralysis). Name tools well, describe them correctly, and add usage examples.
- Use subagent/multi-agent architectures specifically for: diverse workloads, long tool outputs that need distillation, very long tasks, or parallelizable work. The goal is keeping the root agent's context clean.
- Context persistence is the hardest problem: match the memory strategy to the agent type (chatbot history, entity-specific logs, scratchpads for multi-agent communication, todo lists for task tracking).
- Always set up evaluation infrastructure: log traces, replay prompts with numerical scoring, and experiment with changes. When diagnosing issues, ask "why might I make that same mistake given this context?"

## One-Line Summary

Design agentic systems by empathizing with the agent -- define what it needs to observe, act on, and remember as if you were doing the task yourself, then use classical AI frameworks (PEAS, MDP) and rigorous evaluation to refine it.

## Topics

agentic systems, prompt engineering, multi-agent architecture, context management, LLM evaluation, developer practices
