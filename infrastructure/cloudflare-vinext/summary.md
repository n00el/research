# How We Rebuilt Next.js with AI in One Week

**Author:** Steve Faulkner (Cloudflare)
**Date:** 2026-02-24
**Source:** https://blog.cloudflare.com/vinext/

## Key Takeaways

- vinext is a clean reimplementation of the Next.js API surface built as a Vite plugin, not a wrapper or adapter, achieving 94% API coverage and drop-in compatibility with existing `app/`, `pages/`, and `next.config.js`.
- With Vite 8 / Rolldown, production builds are 4.4x faster and client bundles are 57% smaller than Next.js 16 with Turbopack, because Vite's bundler eliminates framework-specific toolchain overhead.
- Traffic-aware Pre-Rendering (TPR) uses Cloudflare zone analytics to pre-render only the pages that actually receive traffic, turning 30-minute static builds of 100k pages into seconds by leveraging power-law distributions.
- The entire project was built by one engineer using AI (800+ Claude sessions, ~$1,100 in API tokens), with quality enforced by 1,700+ unit tests, 380 E2E tests, and CI gating — demonstrating that well-specified reimplementation problems are ideal for AI-assisted development.
- ~95% of vinext is platform-agnostic Vite code, making it deployable beyond Cloudflare Workers — a proof-of-concept ran on Vercel in under 30 minutes.
- The project sidesteps the fragility of OpenNext's reverse-engineering approach by reimplementing the framework contract directly, avoiding breakage from unpredictable Next.js internals changes between versions.

## One-Line Summary

Cloudflare built a drop-in Next.js replacement on Vite in one week using AI, proving that well-specified framework contracts can be reimplemented faster, leaner, and platform-agnostic when you skip the original toolchain entirely.

## Topics

cloudflare workers, vite, nextjs, ai-assisted development, frontend frameworks, serverless deployment
