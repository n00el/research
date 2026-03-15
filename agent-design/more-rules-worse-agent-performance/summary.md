# The more rules i wrote for my agents, the worse they performed.

**Author:** Vox (@Voxyz_ai)
**Date:** Mar 12, 2026
**Source:** https://x.com/Voxyz_ai/status/2032140911585739254

## Key Takeaways

- Agent instruction files (AGENTS.md, SOUL.md, etc.) that exceed the bootstrapMaxChars limit get silently trimmed -- rules at the bottom are dropped without any error or warning, and the agent never knows they existed.
- Adding more rules to fix rule-following failures creates a dead loop: more rules = larger workspace = more trimming = worse performance = even more rules added.
- The leanest agent workspace (1,180 tokens) had the highest accuracy; the heaviest (4,120 tokens) regularly ignored formatting rules -- same model, different context visibility.
- Unused skill descriptions are "dead tokens" that waste context budget and push important rules out of the visible window.
- Four optimization steps: kill unused skill descriptions, relocate historical/expired rules to memory/, merge duplicate tool instructions, and re-scan to verify under 5,000 tokens per agent.
- The formula for agent quality is `rule quality x rule visibility` -- 40 visible core rules beat 100 rules where 40 are trimmed.
- Context management (what the agent actually hears) matters more than prompt writing (what you say) for production agent reliability.

## One-Line Summary

Agent instructions that exceed the context bootstrap limit get silently trimmed, so adding more rules paradoxically degrades performance -- the fix is ruthless workspace slimming, not better prompts.

## Topics

AI agents, context engineering, agent configuration, workspace optimization, AGENTS.md, production debugging
