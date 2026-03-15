# EvoSkill: Automated Skill Discovery for Multi-Agent Systems

**Authors:** Salaheddin Alzubi, Noah Provenzano, Jaydon Bingham, Weiyuan Chen, Tu Vu
**Date:** 3 Mar 2026
**Source:** https://arxiv.org/pdf/2603.02766

## Key Takeaways

- EvoSkill is a self-evolving framework that automatically discovers and refines reusable agent skills through iterative failure analysis, without modifying the underlying model weights.
- The system uses three collaborating agents: an Executor that runs tasks, a Proposer that diagnoses failures and suggests skill improvements, and a Skill-Builder that materializes proposals into structured skill folders (SKILL.md + optional scripts).
- A Pareto frontier of agent programs governs selection: candidate skills are retained only if they improve held-out validation performance, preventing regression.
- On OfficeQA (U.S. Treasury document reasoning), EvoSkill improves exact-match accuracy from 60.6% to 67.9% (+7.3%) using the skill-merge configuration that combines skills from independent runs.
- On SealQA (search-augmented QA with noisy retrieval), EvoSkill achieves a 12.1% absolute gain (26.6% to 38.7%), discovering a search-persistence-protocol that enforces exhaustive search before committing to answers.
- Skills transfer zero-shot across tasks: the search-persistence-protocol evolved on SealQA improves BrowseComp accuracy by 5.3% (43.5% to 48.8%) without any modification.
- The framework builds on Feedback Descent, applying textual feedback (not scalar rewards) to guide iterative skill mutations tracked via git branches representing program lineage.
- Discovered skills are interpretable and domain-relevant (e.g., data extraction verification for Treasury tables, quantitative analysis methodology) rather than opaque prompt optimizations.
- Only small training subsets (5-15% of benchmark data) are needed; the 10% split provides the best cost-efficiency tradeoff for individual runs.
- The skill-merge strategy (combining unique skills across independent evolution runs) consistently outperforms any single run, indicating complementary failure mode coverage.

## One-Line Summary

EvoSkill automatically discovers and refines structured, reusable agent skills through iterative failure analysis and a Pareto frontier selection loop, achieving significant accuracy gains on grounded reasoning and search-augmented QA benchmarks while demonstrating zero-shot skill transfer across tasks.

## Topics

agent skills, self-improving agents, evolutionary optimization, feedback descent, coding agents, skill discovery, transfer learning, claude code
