# SWE-CI: Alibaba's AI Code Maintenance Benchmark

**Author:** Alex Prompter (@alex_prompter)
**Date:** March 7, 2026
**Source:** https://x.com/alex_prompter/status/2030331477918126286
**Stats:** 2,301 likes, 382 retweets, 110 replies, 1,451 bookmarks, ~242,000 views

---

The post covers Alibaba's research on AI coding agents and long-term code maintenance, introducing a benchmark called **SWE-CI** that evaluates how well AI models maintain existing codebases over time -- as opposed to single-task "snapshot" benchmarks like HumanEval or SWE-bench.

## Study Scope

Alibaba tested AI coding agents on **100 real codebases** over **233 days**, evaluating long-term maintenance behavior rather than one-off code generation.

## Central Finding

**75% of AI models break previously working code** during maintenance tasks. This highlights that most AI can *write* code but struggles to *maintain* it without introducing regressions.

## SWE-CI vs. Snapshot Benchmarks

Unlike snapshot benchmarks (HumanEval, SWE-bench) that test isolated coding tasks, SWE-CI measures continuous integration and long-term code maintenance quality, specifically tracking regression rates over time.

## Model Performance

Only **Claude Opus 4** reportedly maintains above a **50% zero-regression rate**, meaning it is the only model tested that avoids breaking existing functionality more than half the time during maintenance tasks.

## Key Data Points

- **100 real codebases** tested
- **233 days** of continuous evaluation
- **75%** of AI models break previously working code
- **Only Claude Opus 4** exceeds 50% zero-regression rate

## Implication

The industry's standard coding benchmarks are insufficient -- being able to generate correct code in isolation is very different from being able to safely maintain and evolve a codebase over time.
