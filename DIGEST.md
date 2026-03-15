# Research Article Digest

**57 articles** across AI agent development, agentic coding, Claude Code, MCP, and industry trends.

---

## Table of Contents

1. [Claude Code Setup & Best Practices](#claude-code-setup--best-practices) (8)
2. [Claude Cowork & Desktop Agents](#claude-cowork--desktop-agents) (4)
3. [Skills, Context Engineering & Knowledge Architecture](#skills-context-engineering--knowledge-architecture) (6)
4. [Agentic Engineering Practices](#agentic-engineering-practices) (6)
5. [Agent Architecture & Building Agents](#agent-architecture--building-agents) (8)
6. [Harness Engineering & Automated Code Quality](#harness-engineering--automated-code-quality) (7)
7. [Programmatic Tool Calling & MCP](#programmatic-tool-calling--mcp) (4)
8. [AI-Assisted Development & Code Quality](#ai-assisted-development--code-quality) (5)
9. [AI Industry, Market Disruption & Trends](#ai-industry-market-disruption--trends) (6)
10. [AI & Work: Productivity, Skills & Career](#ai--work-productivity-skills--career) (3)

---

## Claude Code Setup & Best Practices

### CLAUDE.md Masterclass: From Start to Pro-Level User
`claudemd-masterclass-from-start-to-pro/` | Joe Njenga, January 28, 2026
> CLAUDE.md is a system-prompt-level configuration file with a hard instruction budget (~100-150 usable slots after Claude Code's own system prompt). Keeping it lean and hierarchical, with progressive disclosure to separate files, paired with hooks for enforcement and subagents for isolated execution, is the key to reliable automation.
Tags: `claude code`, `claude.md`, `prompt engineering`, `agentic workflows`

### How to Set Up Claude the Right Way
`how-to-set-up-claude-the-right-way/` | Ruben Hassid, February 20, 2026
> Power users and founders are silently migrating from ChatGPT to Claude, driven by practical daily workflow fit rather than hype. The switch sticks only after proper initial setup — suggesting Claude's advantage is real but gated behind configuration.
Tags: `claude`, `chatgpt`, `ai workflows`, `power users`

### Best Practices for Claude Code (100x Powerful)
`claude-code-best-practices-100x-powerful/` | Meer, February 27, 2026
> Effective Claude Code usage is fundamentally about context window discipline — using Plan Mode before complex tasks, running parallel sessions via git worktrees, treating CLAUDE.md as a curated rulebook, and converting repeated workflows into reusable skills.
Tags: `claude code`, `context window`, `prompt engineering`, `developer productivity`

### Lessons from Building Claude Code: Prompt Caching Is Everything
`lessons-from-claude-code-prompt-caching/` | Thariq, February 19, 2026
> Agentic LLM products must be architected around prompt caching from the ground up. Ordering content static-first/dynamic-last, never mutating cached prefixes, and keeping tool sets stable can reduce long Opus session costs from $50-100 down to $10-19.
Tags: `prompt caching`, `agent architecture`, `cost optimization`, `context management`

### Lessons from Building Claude Code: Seeing Like an Agent
`lessons-claude-code-seeing-like-an-agent/` | Thariq Shihipar, February 27, 2026
> Agent tool design is an empirical discipline: keep the tool count minimal (~20 in Claude Code), reshape the action space to match evolving model capabilities, and prefer progressive context discovery over upfront instrumentation. Replacing RAG with agent-driven Grep was a key unlock.
Tags: `agent design`, `tool design`, `progressive disclosure`, `llm engineering`

### Run Prompts on a Schedule — Claude Code Scheduled Tasks
`claude-code-scheduled-tasks-docs/` | Anthropic, 2026
> Claude Code's `/loop` command enables in-session recurring prompts with configurable intervals and one-time reminders via natural language. Tasks are session-scoped only (lost on exit), with a 50-task limit and 3-day auto-expiry.
Tags: `claude code`, `scheduled tasks`, `cron`, `automation`

### Grep Is Dead: How I Made Claude Code Actually Remember Things
`grep-is-dead-claude-code-remember-things/` | Artem Zhutov, March 2, 2026
> Build persistent memory for Claude Code by indexing your Obsidian vault and session history with QMD's BM25/semantic search. A post-session hook auto-exports conversations, and a /recall skill provides temporal, topic-based, and graph-based retrieval across sessions.
Tags: `claude code`, `obsidian`, `qmd`, `memory`, `context management`

### Grep Is Dead: QMD Recall for Claude Code
`grep-is-dead-claude-code-qmd-recall/` | Artem Zhutov, March 2, 2026
> A persistent memory system for Claude Code using QMD to index Obsidian vaults, enabling cross-session context retrieval via a custom /recall skill. Over 700 conversations exported and indexed, with a 42-minute video walkthrough of the full setup.
Tags: `claude code`, `QMD`, `obsidian`, `persistent memory`, `developer tools`

---

## Claude Cowork & Desktop Agents

### How to Set Up Claude Cowork the Right Way
`cowork-setup-guide/` | Nav Toor, February 25, 2026
> Cowork's biggest leverage comes from preparing context files (about-me.md, brand-voice.md) rather than crafting better prompts. Appending "ask me clarifying questions first" and using folder-level instructions enables autonomous 20-minute work sessions covering ~60% of knowledge work.
Tags: `claude cowork`, `desktop agents`, `context engineering`, `knowledge work`

### How to Set Up Claude Cowork (to Level Up from ChatGPT)
`claude-cowork-setup-guide-2/` | Ruben Hassid, March 5, 2026
> Cowork's release caused $830B in software stock losses. Its key differentiator is the AskUserQuestion feature — Claude prompts you with interactive forms instead of you crafting prompts. A structured folder system replaces elaborate prompting for 80% of tasks.
Tags: `claude cowork`, `anthropic`, `productivity`, `AI workflow`

### I Tested All 21 Claude Cowork Plugins — Tier List
`cowork-plugin-tier-list/` | Nav Toor, February 26, 2026
> Data Analysis, Productivity, and Sales are S-tier plugins worth installing immediately. Plugins are just markdown files, making custom plugin creation trivially easy. A $20/month subscription now does ~40% of what $150/month enterprise SaaS seats do, triggering the "SaaSpocalypse."
Tags: `claude cowork`, `ai plugins`, `saas disruption`, `enterprise ai`

### Claude Cowork Scheduled Tasks for Non-Techies
`claude-cowork-scheduled-tasks-non-techies/` | Nick Spisak, March 7, 2026
> Scheduled Tasks let non-technical users create recurring AI automations using plain English — weekly competitive research, daily inbox triage, automated follow-ups. Context files are the highest-leverage multiplier, making outputs personalized instead of generic.
Tags: `claude cowork`, `scheduled tasks`, `no-code`, `AI automation`

---

## Skills, Context Engineering & Knowledge Architecture

### The Complete Guide to Building Skills for Claude
`complete-guide-building-skills-for-claude/` | Anthropic, January 2026
> Skills are reusable instruction packages with a three-level progressive disclosure system (YAML frontmatter, SKILL.md body, linked files). The description field controls triggering and is the most important part. Published as an open standard portable across AI platforms.
Tags: `claude skills`, `mcp`, `workflow automation`, `agent architecture`

### Skill Graphs > SKILL.md
`skill-graphs-greater-than-skill-md/` | Heinrich, February 18, 2026
> A single skill file hits a ceiling quickly. Connecting many small skill files into a wikilink-based graph with progressive disclosure lets AI agents navigate deep domain knowledge — the difference between an agent that follows instructions and one that understands a domain.
Tags: `skill-graphs`, `context-engineering`, `knowledge-management`, `progressive-disclosure`

### The File System Is the New Database: A Personal OS for AI Agents
`file-system-personal-os-for-ai-agents/` | Muratcan Koylan, February 21, 2026
> Structuring personal knowledge as a modular file-based OS with progressive context disclosure transforms AI from generic chatbot to personalized agent. Module boundaries are loading decisions — splitting identity from brand cut token usage by 40%. Append-only data patterns are non-negotiable.
Tags: `context engineering`, `ai agents`, `file-based architecture`, `prompt engineering`

### Your Company Is a Filesystem
`your-company-is-a-filesystem/` | Eli Mernit, February 10, 2026
> The most effective AI agent architecture reduces to two components: the filesystem as state and an LLM as orchestrator. Modeling company operations as files allows agents to solve business problems through simple reads and writes, making state transparent and inspectable.
Tags: `ai agents`, `filesystem architecture`, `agent design patterns`, `state management`

### Context Rot: How Increasing Input Tokens Impacts LLM Performance
`context-rot-increasing-input-tokens-llm-performance/` | Kelly Hong et al. (Chroma), July 14, 2025
> LLMs fail increasingly and non-uniformly as context grows, even on trivial tasks like word replication. Counterintuitively, shuffling sentences improves retrieval. How you structure information in the context window matters more than simply fitting it all in.
Tags: `long context`, `context engineering`, `LLM evaluation`, `attention mechanisms`

### Everything is Context: Agentic File System Abstraction
`arxiv-2512-05470/` | Xiwei Xu et al., December 5, 2025
> A Unix-inspired "everything is a file" abstraction for context engineering, with a virtual filesystem interface for heterogeneous context sources. Defines a three-component pipeline (Constructor, Updater, Evaluator) and seven memory types, implemented in the open-source AIGNE framework.
Tags: `context engineering`, `file system abstraction`, `LLM-as-OS`, `memory management`, `MCP`

---

## Agentic Engineering Practices

### Just Talk To It — The No-BS Way of Agentic Engineering
`just-talk-to-it-agentic-engineering/` | Peter Steinberger, October 14, 2025
> The best agentic engineering workflow is radically simple — run multiple CLI instances in parallel, write short prompts, and skip the elaborate tooling ecosystem. "Blast radius" thinking (estimating how many files a change touches) is the core planning heuristic. MCPs are mostly marketing theater vs CLIs.
Tags: `agentic engineering`, `codex cli`, `claude code`, `developer workflow`

### How To Be A World-Class Agentic Engineer
`how-to-be-world-class-agentic-engineer/` | sysls, March 3, 2026
> World-class agentic engineering is about minimalist tooling, precise context management, and disciplined task contracts. Separate research from implementation with fresh agents, exploit sycophancy via adversarial multi-agent patterns, and keep CLAUDE.md as a minimal conditional directory.
Tags: `agentic engineering`, `context management`, `best practices`, `minimalism`

### A Simple Framework to Build Agentic Systems That Just Works
`simple-framework-agentic-systems/` | AVB, February 28, 2026
> Design agentic systems by empathizing with the agent — define observations, expected outcomes, and tools as if you were doing the task yourself. Minimize the action space to prevent "agentic dementia," lazy-load context, and match memory strategy to agent type.
Tags: `agentic systems`, `prompt engineering`, `multi-agent architecture`, `context management`

### 2026 Agentic Coding Trends Report
`anthropic-2026-agentic-coding-trends-report/` | Anthropic, 2026
> Engineers are shifting from writing code to orchestrating agents, with multi-agent architectures replacing single-agent workflows. Despite 60% AI usage, only 0-20% of tasks are fully delegated. 27% of AI-assisted work consists of tasks that wouldn't have been done at all without AI.
Tags: `agentic coding`, `multi-agent systems`, `developer productivity`, `human-ai collaboration`

### I Want to Become an AI Engineer (Full Course)
`ai-engineer-full-course-hoeem/` | hoeem, March 5, 2026
> AI engineering has three layers: prompt engineering (foundation) → context engineering (multiplier) → intent engineering (strategic). Intent engineering, with a 7-component schema (objective, outcomes, health metrics, constraints, delegation protocol, stop rules), is the antidote to vibe coding.
Tags: `AI engineering`, `prompt engineering`, `context engineering`, `intent engineering`

### What Spec-Driven Development Gets Wrong
`what-spec-driven-development-gets-wrong/` | Augment Code, February 23, 2026
> Spec-driven development will fail unless agents actively write back to the spec as reality diverges from the plan. Bidirectional specs — where humans describe intent and agents update findings — turn documentation from a stale artifact into a living record of what was actually built.
Tags: `AI agents`, `spec-driven development`, `documentation`, `coding agents`

---

## Agent Architecture & Building Agents

### Agentic Software Engineering
`agentic-software-engineering/` | Ashpreet Bedi, March 6, 2026
> Production agent engineering requires treating agents as distributed systems with six pillars: durability, isolation, governance, persistence, scale, and composability. The work is 40% agent dev + 40% system design + 20% security engineering. Most products stall at the "Serve" stage.
Tags: `agentic software`, `production agents`, `distributed systems`, `system design`

### The 5 Levels of Agentic Software
`five-levels-of-agentic-software/` | Ashpreet Bedi, February 20, 2026
> Build agentic software incrementally across five levels: tools → storage/knowledge → memory/learning → multi-agent teams → production systems. Most teams fail by jumping straight to multi-agent orchestration when a single well-instructed agent with good tools would suffice.
Tags: `agentic architecture`, `ai agents`, `rag`, `multi-agent systems`

### AI Agents 101
`ai-agents-101-vasuman-comprehensive-guide/` | vas, January 15, 2026
> A practitioner's guide emphasizing the perception-decision-action loop, orchestration-layer tool execution, and the 80/20 rule: automate routine decisions while routing complex ones to humans. Build sequence: goal → data needs → tools → test → logic → orchestration → guardrails.
Tags: `ai agents`, `agent architecture`, `orchestration`, `guardrails`

### How We Built Secure, Scalable Agent Sandbox Infrastructure
`agent-sandbox-infrastructure/` | Larsen Cundric, February 27, 2026
> Agents executing arbitrary code should run in zero-secret, disposable sandboxes (Unikraft micro-VMs booting in <1s) that talk to a credential-holding control plane. The extra network hop is negligible compared to LLM latency, making the security tradeoff strongly favorable.
Tags: `agent infrastructure`, `sandbox security`, `micro-vms`, `zero-trust compute`

### My Ralph Loop Setup for Long Running AI Agents
`ralph-loop-setup-long-running-ai-agents/` | Dan, February 23, 2026
> Long-running AI coding agents become practical when you replace a single massive context window with a stateless loop persisting progress in git commits. A structured PRD with discrete verifiable tasks is the most critical input; mid-run steering is possible via STEERING.md.
Tags: `ai agents`, `developer workflows`, `automation`, `docker`

### Pal: A Personal Context-Agent That Learns How You Work
`pal-personal-context-agent-learns-how-you-work/` | Ashpreet Bedi, March 6, 2026
> Pal replaces RAG-style vector search with a "context graph" — navigating across SQL, files, email, and calendar while learning which retrieval strategies work best. Scheduled background tasks (daily briefings, inbox digests) turn it from a reactive tool into a compounding system.
Tags: `AI agents`, `context agents`, `RAG alternatives`, `personal assistant`

### How to Build a Data Agent in 2026
`how-to-build-a-data-agent-2026/` | Jamie Quint, March 5, 2026
> Replace semantic layers with dynamic context agents and learned "quirks" (user corrections stored with embeddings). A context sub-agent reads DBT models at runtime instead of pre-defining business concepts. This reduced a 4-5 analyst hiring plan to just one hire.
Tags: `AI agents`, `data engineering`, `modern data stack`, `semantic layer`

### Annotated Screenshots for Agent-Browser
`annotated-screenshots-agent-browser/` | Chris Tate, February 19, 2026
> Annotated screenshots overlay numbered labels on interactive elements, bridging the gap between visual page context and actionable structure in browser agents. Available via a single `--annotate` flag, solving a core grounding problem.
Tags: `browser agents`, `accessibility`, `web automation`, `agent tooling`

---

## Harness Engineering & Automated Code Quality

### Harness Engineering: Leveraging Codex in an Agent-First World
`openai-harness-engineering-blog/` | OpenAI, February 2026
> Three engineers shipped ~1M lines of 100% agent-generated code in 5 months via ~1,500 PRs. The moat is the control infrastructure — linters with remediation instructions, CI, progressive docs structure, and feedback loops — not the code itself. Repository optimized for agent legibility first.
Tags: `harness engineering`, `openai`, `codex`, `agentic coding`, `CI/CD`

### 8 Principles of Harness Engineering for AI Coding Agents
`openai-harness-engineering-insights/` | Muratcan Koylan, March 5, 2026
> Engineers become environment designers: progressive disclosure beats monolithic docs, mechanical enforcement (linters, CI) is more reliable than natural-language instructions, and a velocity-first merge philosophy addresses test failures post-merge rather than blocking on them.
Tags: `harness engineering`, `AI agents`, `developer workflow`, `codex`

### Harness Engineering Is Cybernetics
`harness-engineering-is-cybernetics/` | George, March 7, 2026
> Harness engineering is the same cybernetic pattern as Watt's governor — you stop turning the valve and start steering. When agents produce bad output, the problem is almost never capability; it's that domain knowledge hasn't been externalized into machine-readable form.
Tags: `AI agents`, `harness engineering`, `cybernetics`, `feedback loops`

### AutoHarness: Automatic Harness Synthesis for LLM Agents
`autoharness-automatic-harness-synthesis/` | DAIR.AI, March 7, 2026
> LLMs can auto-synthesize their own code harnesses through iterative refinement. Gemini-2.5-Flash with AutoHarness beat larger models while reducing costs, proving that harness quality matters more than model size for agent performance.
Tags: `autoharness`, `agent scaffolding`, `LLM agents`, `cost efficiency`

### Code Factory: Auto Write and Review 100% of Your Code
`code-factory-agent-auto-write-review-code/` | Ryan Carson, February 16, 2026
> To let agents write and review all your code, build a deterministic control plane: risk-tiered policy gates, current-head SHA discipline, automated remediation loops, and browser evidence as machine-verifiable CI artifacts. Never bypass policy gates.
Tags: `agent-driven development`, `ci/cd automation`, `code review agents`, `risk-based merge policy`

### How to Force Your Agent to Obey Your Design System
`enforcing-design-system-compliance/` | Ryan Carson, March 3, 2026
> A 5-layer deterministic system (canonical docs → agent routing via SKILL.md → custom lint rules → pre-commit hooks → CI gates) treats design systems as enforceable contracts rather than optional guidance. Framework-agnostic and agent-agnostic.
Tags: `design systems`, `AI agents`, `linting`, `CI/CD`, `frontend`

### I Made Claude and Codex Argue Until My Code Plan Was Actually Good
`claude-codex-iterative-plan-review/` | Aseem Shrey, February 20, 2026
> Pitting two frontier models against each other in a structured review loop catches far more plan-level defects than any single-model workflow. In a real-world test, 3 automated rounds caught 14 issues and elevated a rough draft to a production-grade spec with zero manual effort.
Tags: `adversarial ai review`, `codex cli`, `multi-model workflows`, `implementation planning`

---

## Programmatic Tool Calling & MCP

### Give Claude a Computer: Programmatic Tool Calling
`programmatic-tool-calling/` | Lance Martin, February 27, 2026
> PTC lets Claude write code that orchestrates tool calls inside a container, eliminating per-call round-trip tax. On web search benchmarks, PTC improved accuracy by 11% while using 24% fewer input tokens. Opus 4.6 with PTC holds #1 on LMArena's Search Arena.
Tags: `programmatic tool calling`, `agent architecture`, `token efficiency`, `tool orchestration`

### Programmatic Tool Calling (Companion Summary)
`programmatic-tool-calling-claude/` | Lance Martin, Anthropic, February 27, 2026
> Part of Anthropic's Advanced Tool Use suite alongside Tool Search (~85% token reduction) and Tool Use Examples (72% → 90% accuracy). Token consumption dropped 37% in a practical budget compliance example. Keep discrete tools for UX, guardrails, and observability needs.
Tags: `anthropic`, `claude`, `tool calling`, `tokens`, `agent architecture`

### Code Mode: Give Agents an Entire API in 1,000 Tokens
`cloudflare-code-mode-mcp/` | Matt Carey (Cloudflare), February 20, 2026
> Cloudflare's Code Mode collapses an entire API into two tools (`search()` and `execute()`), cutting token usage by 99.9% to a fixed ~1,000-token footprint regardless of API size. Agents discover endpoints by writing JavaScript that queries the OpenAPI spec at runtime inside a V8 isolate.
Tags: `mcp`, `ai agents`, `token optimization`, `cloudflare workers`, `api design`

### MCP: The Only Guide You Need to Connect Claude to Everything
`mcp-only-guide-connect-claude-everything/` | witcheer, March 7, 2026
> A practical guide to using MCP (Model Context Protocol) to connect Claude to external tools, databases, and APIs, eliminating manual copy-paste workflows. High bookmark-to-like ratio signals strong practical reference value.
Tags: `MCP`, `model context protocol`, `tool integration`, `developer tools`

---

## AI-Assisted Development & Code Quality

### Vibe Coding 2.0: 18 Rules to Be the Top 1% Builder
`vibe-coding-2-18-rules-top-1-percent-builder/` | Harshil Tomar, February 24, 2026
> The best vibe coders ruthlessly identify what not to build, choosing managed services (Clerk, Supabase, Vercel) and opinionated stacks to eliminate decision fatigue. Use version control with feature branches even as a solo developer — AI agents make confident mistakes at speed.
Tags: `vibe coding`, `mvp strategy`, `developer tooling`, `startup engineering`

### How Cloudflare Rebuilt Next.js with AI in One Week (vinext)
`cloudflare-vinext/` | Steve Faulkner (Cloudflare), February 24, 2026
> One engineer used 800+ Claude sessions (~$1,100 in API tokens) to reimplement the Next.js API surface as a Vite plugin, achieving 94% compatibility with 4.4x faster builds and 57% smaller bundles. Well-specified reimplementation problems are ideal for AI-assisted development.
Tags: `cloudflare workers`, `vite`, `nextjs`, `ai-assisted development`

### Your LLM Doesn't Write Correct Code — It Writes Plausible Code
`llm-doesnt-write-correct-code-plausible/` | Horoshi, March 6, 2026
> LLM-generated code contains fundamental algorithmic flaws only domain experts catch. An AI SQLite reimplementation was 20,171x slower despite compiling and passing all tests. METR study: experienced devs using AI were 19% slower but believed AI accelerated them.
Tags: `LLM code quality`, `semantic bugs`, `AI coding risks`, `benchmarks`

### SWE-CI: Alibaba's AI Code Maintenance Benchmark
`alibaba-swe-ci-ai-code-maintenance/` | Alex Prompter, March 7, 2026
> 75% of AI models break previously working code during long-term maintenance (100 codebases, 233 days). Only Claude Opus 4 maintains above 50% zero-regression rate. Writing code and maintaining code are fundamentally different skills for AI.
Tags: `benchmarks`, `code maintenance`, `SWE-CI`, `regression testing`

### Chrome Memory Snapshot + Cursor AI for Performance Debugging
`chrome-memory-snapshot-cursor-ai-perf/` | ian, February 20, 2026
> Export a Chrome DevTools memory snapshot and drop it into an AI coding assistant — it writes custom Python analysis scripts that pinpoint exactly what's making your website slow. Turns expert-level debugging into a three-step workflow any developer can do.
Tags: `ai-assisted-debugging`, `web-performance`, `chrome-devtools`, `memory-profiling`

---

## AI Industry, Market Disruption & Trends

### We Will All Be Digital Gods: The Death of Apps and the Rise of the Meta-App
`digital-gods-death-of-apps-rise-of-meta-app/` | Zach Lloyd, February 27, 2026
> "Meta-apps" like Claude Code and Warp go directly from user intent to outcome, bypassing specific apps. Traditional app frontends have no moat anymore — users can generate better, personalized UIs on demand. Data availability, not app frontends, is the real battleground.
Tags: `meta-apps`, `ai agents`, `saas disruption`, `data portability`

### 10 Years Building Vertical Software: My Perspective on the Selloff
`10-years-building-vertical-software-selloff/` | Nicolas Bustamante, February 16, 2026
> LLMs are destroying the five moats (learned interfaces, encoded workflows, data accessibility, talent scarcity, bundling) that kept vertical software defensible. Competition in each vertical will explode from 3 incumbents to 300+ entrants. The only durable moats: proprietary data, regulatory lock-in, and transaction embedding.
Tags: `vertical saas`, `llm disruption`, `software moats`, `enterprise valuations`

### The Rise of the Agent Builder
`rise-of-the-agent-builder/` | Zach Lloyd (Warp), March 4, 2026
> Warp's CEO argues every 50+ person company will need a dedicated "Agent Builder" role within a year to systematically replace SaaS tools with custom AI agents. Coding agents generalize surprisingly well into general-purpose automation agents.
Tags: `AI agents`, `SaaS replacement`, `agent builder`, `enterprise AI`

### The Vibe Coding Trap
`the-vibe-coding-trap/` | Max Musing, February 12, 2026
> AI makes building software nearly free but makes owning it relatively more expensive, because each engineering hour now has higher opportunity cost. SaaS products that absorb operational complexity (Stripe, Cloudflare) will become more valuable, not less — "mediocre SaaS" is genuinely threatened.
Tags: `vibe-coding`, `saas`, `build-vs-buy`, `software-ownership`

### GPT-5.4 Thinking and Pro Rolling Out Now
`openai-gpt54-thinking-pro-rollout/` | OpenAI, March 5, 2026
> OpenAI launches GPT-5.4 as a unified frontier model merging the o-series (reasoning) and GPT series (general) into one model line, with agentic workflows elevated to first-class capability alongside reasoning and coding.
Tags: `openai`, `gpt-5.4`, `frontier models`, `agentic workflows`

### Codex Security: Now in Research Preview
`openai-codex-security-research-preview/` | OpenAI, March 6, 2026
> An AI-powered security agent that generates project-specific threat models and validates vulnerabilities in sandboxes. Scanned 1.2M commits in 30 days finding 792 critical issues, with 84% noise reduction and 50%+ false positive reduction vs traditional scanners. Found real CVEs in GnuPG, Chromium, OpenSSH.
Tags: `openai`, `codex`, `security`, `vulnerability scanning`, `agentic tools`

---

## AI & Work: Productivity, Skills & Career

### AI Doesn't Reduce Work — It Intensifies It
`ai-doesnt-reduce-work-it-intensifies-it/` | Aruna Ranganathan & Xingqi Maggie Ye, February 9, 2026
> AI adoption triggers a self-reinforcing cycle of voluntary work intensification — expanding scope, eroding boundaries, and increasing multitasking. Short-term productivity gains mask unsustainable workload creep that leads to cognitive fatigue, impaired judgment, and eventual burnout.
Tags: `ai productivity`, `work intensification`, `burnout`, `organizational design`

### AI Skills Are Not Durable
`ai-skills-are-not-durable/` | Lukas, March 2, 2026
> Every specific AI power-user technique (chain-of-thought prompting, context engineering, custom tooling) has been obsoleted within months by new model releases. The only consistently valuable skills are having good ideas and communicating them clearly.
Tags: `ai skills`, `prompt engineering`, `ai hype`, `clear thinking`

### The Skills That Will Be Worth $500/Hour in 2027
`skills-worth-500-per-hour-in-2027-free-to-learn/` | Zephyr, February 27, 2026
> The most lucrative skills of 2027 will belong to people who learn to orchestrate and deploy AI systems today — agent development, workflow automation, and system integration — while everyone else worries about being replaced. The window to build expertise is now.
Tags: `ai skills`, `career strategy`, `ai agents`, `future of work`
