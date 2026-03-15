# How We Rebuilt Next.js with AI in One Week

**Author:** Steve Faulkner (Cloudflare)
**Date:** 2026-02-24
**Source:** https://blog.cloudflare.com/vinext/
**Tags:** AI, Cloudflare Workers, JavaScript, Open Source, Performance

---

## TL;DR

One engineer + AI rebuilt the most popular front-end framework from scratch in under a week. The result, **vinext** (pronounced "vee-next"), is a drop-in replacement for Next.js built on Vite that deploys to Cloudflare Workers with a single command. Builds up to 4x faster, client bundles up to 57% smaller. Total cost: ~$1,100 in Claude API tokens.

## The Next.js Deployment Problem

Next.js is the most popular React framework, but has a deployment problem in the serverless ecosystem. The tooling is bespoke (Turbopack), and deploying to Cloudflare, Netlify, or AWS Lambda requires reshaping the build output.

**OpenNext** was built to solve this by reverse-engineering Next.js's build output, but it's fragile — unpredictable changes between versions require constant fixes. Next.js is working on a first-class adapters API, but even with adapters you're still on the Turbopack toolchain, and `next dev` runs exclusively in Node.js with no way to plug in a different runtime.

## What Is vinext?

A **clean reimplementation** of the Next.js API surface on Vite directly — not a wrapper or adapter.

```
npm install vinext
```

Replace `next` with `vinext` in your scripts. Existing `app/`, `pages/`, and `next.config.js` work as-is.

```
vinext dev     # Development server with HMR
vinext build   # Production build
vinext deploy  # Build and deploy to Cloudflare Workers
```

Implements: routing, server rendering, React Server Components, server actions, caching, middleware. All built as a Vite plugin. Vite output runs on any platform via the Vite Environment API.

## Benchmarks

**Production build time** (33-route App Router app):

| Framework | Mean | vs Next.js |
|-----------|------|------------|
| Next.js 16.1.6 (Turbopack) | 7.38s | baseline |
| vinext (Vite 7 / Rollup) | 4.64s | **1.6x faster** |
| vinext (Vite 8 / Rolldown) | 1.67s | **4.4x faster** |

**Client bundle size** (gzipped):

| Framework | Gzipped | vs Next.js |
|-----------|---------|------------|
| Next.js 16.1.6 | 168.9 KB | baseline |
| vinext (Rollup) | 74.0 KB | **56% smaller** |
| vinext (Rolldown) | 72.9 KB | **57% smaller** |

*Benchmarks measure compilation/bundling speed only, not production serving. [Full methodology](https://benchmarks.vinext.workers.dev) is public.*

## Deploying to Cloudflare Workers

Single command: `vinext deploy` — builds, auto-generates Worker config, deploys. Both App Router and Pages Router work with full client-side hydration.

**ISR via Cloudflare KV:**
```js
import { KVCacheHandler } from "vinext/cloudflare";
import { setCacheHandler } from "next/cache";
setCacheHandler(new KVCacheHandler(env.MY_KV_NAMESPACE));
```

Caching is pluggable — KV is the default, R2 for large payloads, Cache API improvements coming.

**Live examples:**
- [App Router Playground](https://app-router-playground.vinext.workers.dev)
- [Hacker News clone](https://hackernews.vinext.workers.dev)
- [App Router minimal](https://app-router-cloudflare.vinext.workers.dev)
- [Pages Router minimal](https://pages-router-cloudflare.vinext.workers.dev)

Also supports Cloudflare Agents (Durable Objects, AI bindings) without workarounds like `getPlatformProxy` — entire app runs in workerd during both dev and deploy.

## Frameworks Are a Team Sport

~95% of vinext is pure Vite (routing, module shims, SSR pipeline, RSC integration — none Cloudflare-specific). Cloudflare is looking to work with other hosting providers. Got a proof-of-concept working on Vercel in <30 minutes. Open-source, PRs from other platforms welcome.

## Status: Experimental

Not yet battle-tested at scale. But:
- 1,700+ Vitest tests
- 380 Playwright E2E tests
- Tests ported from Next.js test suite and OpenNext's Cloudflare conformance suite
- 94% Next.js 16 API surface coverage
- Already running in production at [CIO.gov](https://www.cio.gov/) via National Design Studio

[What's not supported](https://github.com/cloudflare/vinext#whats-not-supported-and-wont-be) and [known limitations](https://github.com/cloudflare/vinext#known-limitations) are documented.

## Traffic-aware Pre-Rendering (TPR)

Next.js pre-renders every page in `generateStaticParams()` at build time. 10,000 product pages = 10,000 renders, even if 99% never get visited. This causes 30-minute builds.

**TPR** queries Cloudflare's zone analytics at deploy time and pre-renders only the pages that matter:

```
vinext deploy --experimental-tpr
Building... Build complete (4.2s)
TPR: 12,847 unique paths — 184 pages cover 90% of traffic
TPR: Pre-rendered 184 pages in 8.3s → KV cache
Deploying to Cloudflare Workers...
```

For 100,000 product pages, power law means ~50–200 pages cover 90% of traffic. Everything else falls back to on-demand SSR + ISR caching. No `generateStaticParams()` needed. Viral pages get picked up automatically.

## How They Built It (AI-First Development)

Almost every line written by AI, but passes same quality gates as human code (1,700+ tests, 380 E2E tests, TypeScript via tsgo, linting via oxlint, CI on every PR).

**Process:**
1. Spent hours with Claude in OpenCode to define architecture
2. Define a task → AI writes implementation + tests → run test suite → merge if pass, iterate if not → repeat
3. AI agents for code review — PR opened → agent reviews → agent addresses review comments
4. Used agent-browser for browser-level testing (hydration, client navigation)
5. 800+ sessions in OpenCode, ~$1,100 in Claude API tokens

**Why this problem suited AI:**
- Next.js is well-specified (extensive docs, massive training data)
- Elaborate test suite to verify against mechanically
- Vite handles the hard parts (HMR, ESM, bundling)
- Models caught up — can hold full architecture in context and reason about module interactions

**Key insight on software layers:**

> "Most abstractions in software exist because humans need help. We couldn't hold the whole system in our heads, so we built layers to manage complexity. AI doesn't have the same limitation. It can hold the whole system in context and just write the code."

## Getting Started

**With AI migration skill:**
```
npx skills add cloudflare/vinext
```
Then tell your AI coding tool: "migrate this project to vinext"

**Manual:**
```
npx vinext init    # Migrate existing Next.js project
npx vinext dev     # Start dev server
npx vinext deploy  # Ship to Cloudflare Workers
```

**Source:** [github.com/cloudflare/vinext](https://github.com/cloudflare/vinext)
