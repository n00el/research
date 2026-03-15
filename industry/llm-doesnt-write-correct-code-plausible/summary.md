# Your LLM Doesn't Write Correct Code. It Writes Plausible Code.

**Author:** Horoshi (Vagabond) / @KatanaLarp
**Date:** March 6, 2026
**Source:** https://x.com/KatanaLarp/status/2029928471632224486

## Key Takeaways

- LLM-generated SQLite reimplementation was 20,171x slower than real SQLite despite compiling and passing all tests
- Semantic bugs (wrong algorithms, missing optimizations) are the real danger — not syntax errors
- An overengineered LLM solution produced 82K lines of Rust for a problem solvable by a single `find` cron job
- METR study: experienced devs using AI were 19% slower but believed AI accelerated them
- Google DORA 2024: every 25% increase in AI adoption correlated with 7.2% decreased delivery stability
- Root cause: LLM sycophancy optimizes for user approval over correctness

## One-Line Summary

LLMs produce code that looks correct and passes tests but contains fundamental algorithmic flaws only domain experts would catch — the danger is semantic bugs, not syntax errors.

## Topics

LLM code quality, semantic bugs, AI coding risks, benchmarks, software engineering
