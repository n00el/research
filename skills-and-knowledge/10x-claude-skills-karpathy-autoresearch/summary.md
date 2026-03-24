# How to 10x your Claude Skills (using Karpathy's autoresearch method)

**Author:** Ole Lehmann (@itsolelehmann)
**Date:** Mar 17, 2026
**Source:** https://x.com/itsolelehmann/status/2033919415771713715

## Key Takeaways

- Karpathy's autoresearch method adapted for Claude skills: an agent loop that makes one small change to a skill prompt, tests it, keeps improvements, and reverts regressions -- fully autonomous
- The key input you provide is a scoring checklist of 3-6 yes/no questions that define what "good" output looks like (e.g., "Does the headline include a specific number?")
- Real result: a landing page copy skill went from 56% to 92% pass rate in 4 rounds -- the agent added specific rules, banned buzzwords, and a worked example
- The system produces a changelog documenting every attempted change and its effect, which future smarter models can use to continue optimization
- The method works on anything scorable: website speed (1100ms to 67ms in 67 rounds), cold outreach, newsletter intros, any repeated prompt
- More than 6 checklist questions causes "gaming" where the skill optimizes for the checklist at the expense of overall quality

## One-Line Summary

An autonomous skill improvement method based on Karpathy's autoresearch that uses a test-score-revise loop with yes/no checklists to automatically optimize any Claude skill's prompt from ~56% to 92%+ reliability.

## Topics

autoresearch, Claude skills, prompt optimization, Karpathy, self-improving agents, evaluation
