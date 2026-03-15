# AI Agents 101

**Author:** vas (@vasuman)
**Date:** January 15, 2026
**Source:** https://x.com/vasuman/status/2011923440769659132

## Key Takeaways

- An agent is a system that acts on goals, not instructions — it needs perception, decision logic, and action interfaces working in a loop.
- Agents excel at repetitive decisions that follow learnable patterns; the best deployments handle 80% of cases autonomously and route the complex 20% to humans.
- Tools are functions the agent calls indirectly through an orchestration layer that validates permissions and parameters before execution.
- Guardrails must be hard limits the agent cannot bypass — prohibited actions, rate limits, and role-based access controls.
- The recommended build sequence is: define goal, list data needs, build tools, test independently, add decision logic, wrap in orchestration, test with real data, then layer on guardrails and validation.
- Memory is essential because agents operate over time across long conversations and multi-step workflows, and it doubles as an audit trail.

## One-Line Summary

A practitioner's guide to building production AI agents, emphasizing the perception-decision-action loop, orchestration-layer tool execution, and the 80/20 rule of automating routine decisions while routing complex ones to humans.

## Topics

ai agents, agent architecture, orchestration, guardrails, enterprise ai, developer guide
