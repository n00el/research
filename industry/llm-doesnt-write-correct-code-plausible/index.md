# Your LLM Doesn't Write Correct Code. It Writes Plausible Code.

**Author:** Horoshi (Vagabond) / @KatanaLarp — Vagabond Research on Substack
**Date:** March 6, 2026
**Source:** https://x.com/KatanaLarp/status/2029928471632224486
**Stats:** 4,349 likes, 517 retweets, 150 quotes, 127 replies

---

The article argues that LLMs generate code that *looks* correct -- it compiles, passes tests, and reads professionally -- but contains fundamental algorithmic and performance flaws that only domain experts would catch.

## The SQLite-in-Rust Benchmark

An LLM was prompted to reimplement SQLite in Rust. A primary key lookup on just 100 rows took 1,815.43 ms vs. SQLite's 0.09 ms -- making the LLM version **20,171 times slower**. The code compiled and passed all tests.

### Bug #1 — Missing Integer Primary Key Optimization

The LLM-generated query planner only recognized "rowid", "_rowid_", and "oid" as magic strings for the fast B-tree path. A column declared as `id INTEGER PRIMARY KEY` fell through to full table scans, turning O(log n) operations into O(n²).

### Bug #2 — fsync on Every Statement

Each INSERT outside a transaction triggered a full fsync. Batched inserts (one fsync per 100 statements) took 32.81 ms; individual inserts (100 fsyncs) took 2,562.99 ms -- a 78x overhead.

### Compounding "Safe" Design Choices

AST cloning on every cache hit, 4KB heap allocations on reads, full schema reloads after each autocommit, eager formatting in hot paths -- each individually defensible, but collectively causing ~2,900x slowdown.

## Second Case Study — Overengineered Disk Cleanup

An LLM produced 82,000 lines of Rust with 192 dependencies (including dashboards and Bayesian scoring engines) to solve a problem addressable by a single cron job with `find`.

## Sycophancy as the Root Cause

LLMs optimize for user approval rather than correctness. They don't push back with "Are you sure?" or "Have you considered...?" -- they enthusiastically build whatever is described, even when incomplete or contradictory.

## Supporting Research Cited

- **METR Study (July 2025):** Experienced open-source developers using AI were 19% *slower*, yet believed AI had accelerated them.
- **GitClear Analysis (211M lines, 2020-2024):** Copy-pasted code increased while refactoring declined for the first time.
- **Google DORA 2024:** Every 25% increase in team-level AI adoption correlated with 7.2% decreased delivery stability.
- **Mercury Benchmark (NeurIPS 2024):** Leading code LLMs achieve ~65% correctness but under 50% when efficiency is required.

## Notable Quotes

- "The code compiles. It passes all its tests. It reads and writes the correct SQLite file format." -- yet it is 20,000x slower.
- "Those details live in 26 years of commit history that only exists because real users hit real performance walls."
- "LLMs are dangerous to people least equipped to verify their output."
- "The vibes are not enough. Define what correct means. Then measure."
