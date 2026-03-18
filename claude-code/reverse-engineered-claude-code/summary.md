# How I Reverse Engineered Claude Code

**Author:** Arinjay Wyawhare (@jaywyawhare)
**Date:** 2026-03-16
**Source:** https://x.com/jaywyawhare/status/2033488305191616875

## Key Takeaways

- Claude Code is a 213 MB Bun single executable containing 9.88 MB of minified JavaScript (7,493 lines) -- the entire application is surprisingly small once you remove the runtime
- The system prompt is assembled at runtime from 15+ modular sections (identity, tone, task management, tool usage, security policy, dynamic sections) using 10,000-20,000 tokens of scaffolding per turn
- "Tengu" is the internal feature flag/telemetry system: 37 feature flags (GrowthBook + Statsig) and 560 telemetry events (OpenTelemetry to Datadog)
- Anthropic ships features into the binary weeks before enabling them publicly -- comparing v2.1.33 to v2.1.76 confirmed gated features in the old version were live in the new one
- The security policy is hardcoded and cannot be overridden; the bash sandbox has a genuine allow/deny mechanism with network restrictions
- The author's core insight: "Claude Code is a prompt delivery system. The code is plumbing. The product is the prompt."

## One-Line Summary

A security researcher extracted and analyzed Claude Code's 9.88 MB JavaScript bundle, revealing a modular 15-section prompt architecture, 37 feature flags, 560 telemetry events, and the conclusion that the binary is just plumbing -- the prompt is the product.

## Topics

Claude Code, reverse engineering, system prompt, feature flags, Tengu, binary analysis
