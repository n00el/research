# Alibaba just proved that AI Coding isn't taking your job

**Author:** Priyanka Vergadia (@pvergadia)
**Date:** 2026-03-16
**Source:** https://x.com/pvergadia/status/2033362617352556980

## Key Takeaways

- Alibaba tested 18 AI agents on 100 real codebases over 233-day evolution cycles using a new benchmark called SWE-CI, shifting evaluation from one-shot correctness to long-term maintainability
- 75% of models broke previously working code during maintenance tasks, accumulating compounding technical debt
- Only Claude Opus 4.5/4.6 maintained a >50% zero-regression rate across the benchmark
- Traditional "snapshot" benchmarks like HumanEval only measure whether code works at a single point in time, missing the critical maintenance dimension
- SWE-CI asks whether code still works after 8 months of continuous evolution with 71 consecutive commits
- Most AI coding agents are "quick-fix artists" that write brittle code passing tests today but creating maintenance nightmares tomorrow

## One-Line Summary

Alibaba's SWE-CI benchmark reveals that while most AI models can write code, almost none can maintain it over long-term evolution cycles -- with 75% breaking previously working code and only Claude Opus achieving >50% zero-regression rates.

## Topics

agent-benchmarks, code-maintenance, SWE-CI, Alibaba, AI-coding, long-term-evaluation
