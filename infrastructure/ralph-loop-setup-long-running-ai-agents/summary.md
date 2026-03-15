# My Ralph Loop setup for long running AI Agents

**Author:** Dan (@d4m1n)
**Date:** February 23, 2026
**Source:** https://x.com/d4m1n/status/2026032801322356903

## Key Takeaways

- The Ralph Loop avoids context window degradation by giving each iteration a fresh context -- state is persisted in text files and git commits, not in the conversation history.
- A structured PRD broken into discrete, verifiable tasks is the most critical input; poorly specified requirements produce bad results regardless of how good the loop is.
- The loop runs inside a Docker sandbox so the AI agent gets full permissions without risking the host machine, and it can run unattended for 30+ hours completing hundreds of tasks.
- Mid-run steering is possible by editing a STEERING.md file that the agent reads at the start of each iteration, allowing priority changes or bug fixes without restarting.
- Every completed task produces a git commit, making rollback trivial -- reverting a commit causes its tests to fail, and the loop will re-attempt the task automatically.
- The workflow shifts the developer's role from writing code to planning, delegating, and reviewing -- the most valuable skill becomes describing requirements clearly rather than typing code faster.
- The approach works best for prototyping, MVPs, automated test generation, migrations, and bulk refactoring; it struggles with pixel-perfect design, novel architecture, and security-critical code.

## One-Line Summary

Long-running AI coding agents become practical when you replace a single massive context window with a stateless loop that persists progress in git commits and picks up fresh tasks each iteration.

## Topics

ai agents, developer workflows, automation, claude code, docker, agentic coding
