# Prioritized Reading List

> For someone building an AI agent platform with Claude Code, MCP integrations, React Native/Expo apps, and personal productivity automation.
>
> 57 articles scored on: direct Claude Code applicability (high), actionable techniques (high), MCP/skills/context/architecture relevance (medium), React Native/Expo relevance (medium), general AI awareness (low).

---

## Quick Start

**If you only have 1 hour, read these 5 articles in this order:**

1. **The Complete Guide to Building Skills for Claude** — `complete-guide-building-skills-for-claude/` — Official Anthropic reference for the entire skills system
2. **CLAUDE.md Masterclass: From Start to Pro** — `claudemd-masterclass-from-start-to-pro/` — How to configure Claude Code's instruction budget properly
3. **Lessons from Building Claude Code: Seeing like an Agent** — `lessons-claude-code-seeing-like-an-agent/` — First-party insights on tool design from the Claude Code team
4. **A Simple Framework to Build Agentic Systems** — `simple-framework-agentic-systems/` — The most practical agent design framework in the collection
5. **Harness Engineering: Leveraging Codex in an Agent-First World** — `openai-harness-engineering-blog/` — How infrastructure (not code) is the real moat in agent-driven development

---

## 🔴 Must Read (13 articles)

### 1. The Complete Guide to Building Skills for Claude
📁 `complete-guide-building-skills-for-claude/` | Why: Official Anthropic guide — the canonical reference for the skills system you're building on
Key takeaway: Skills use three-level progressive disclosure (YAML frontmatter → SKILL.md body → linked files) and the description field is the single most important part because it controls when Claude loads the skill.

### 2. CLAUDE.md Masterclass: From Start to Pro-Level User
📁 `claudemd-masterclass-from-start-to-pro/` | Why: The instruction budget ceiling (~100-150 slots) is a hard constraint that shapes every design decision
Key takeaway: Keep CLAUDE.md lean with universally applicable rules, offload task-specific instructions to separate files, and pair with hooks (enforcement) and subagents (execution).

### 3. Lessons from Building Claude Code: Seeing like an Agent
📁 `lessons-claude-code-seeing-like-an-agent/` | Why: First-party design philosophy from the engineer who built Claude Code — explains why tools are shaped the way they are
Key takeaway: Agent tool design is empirical — minimize the tool count, prefer progressive disclosure over bloated system prompts, and reshape the action space with every model upgrade.

### 4. Best Practices for Claude Code (100x Powerful)
📁 `claude-code-best-practices-100x-powerful/` | Why: Comprehensive playbook of immediately applicable Claude Code workflow patterns
Key takeaway: Context window management is the single most important skill — use /clear, /compact, subagents, and parallel worktree sessions to keep the "whiteboard" clean.

### 5. Lessons from Building Claude Code: Prompt Caching Is Everything
📁 `lessons-from-claude-code-prompt-caching/` | Why: Caching architecture determines whether an agentic product is economically viable at scale
Key takeaway: Order content static-first/dynamic-last, never mutate the system prompt mid-conversation, and treat cache hit rate as a critical infrastructure metric.

### 6. How To Be A World-Class Agentic Engineer
📁 `how-to-be-world-class-agentic-engineer/` | Why: Counterweight to over-engineering — proves minimalist tooling + precise context beats elaborate harnesses
Key takeaway: CLAUDE.md should be a minimal conditional directory ("if X, read this"), one fresh session per task contract beats long-running sessions, and separate research from implementation agents.

### 7. A Simple Framework to Build Agentic Systems That Just Works
📁 `simple-framework-agentic-systems/` | Why: The most actionable general agent design framework — applies to every agent you'll build
Key takeaway: "Put yourself in the agent's shoes" — define observations, expected outcomes, and available tools; lazy-load context; minimize the action space to prevent "agentic dementia."

### 8. The File System Is the New Database: A Personal OS for AI Agents
📁 `file-system-personal-os-for-ai-agents/` | Why: Directly applicable architecture for your file-based agent platform
Key takeaway: Progressive disclosure (routing file → module instructions → data files) cut token usage 40% vs monolithic prompts, and append-only patterns are non-negotiable for AI-managed files.

### 9. Skill Graphs > SKILL.md
📁 `skill-graphs-greater-than-skill-md/` | Why: The next evolution of the skills system — how to handle domains too rich for single files
Key takeaway: Connect many small skill files into a wikilink-based graph with YAML frontmatter descriptions and Maps of Content (MOCs) for progressive discovery.

### 10. Grep Is Dead: How I Made Claude Code Actually Remember Things
📁 `grep-is-dead-claude-code-remember-things/` | Why: You're already running this exact QMD + recall stack — this is the reference implementation
Key takeaway: A post-session hook auto-exports Claude Code conversations into QMD, enabling temporal/topic/graph-based recall that turns notes into active, searchable context.

### 11. Ralph Loop Setup for Long-Running AI Agents
📁 `ralph-loop-setup-long-running-ai-agents/` | Why: Directly applicable pattern for autonomous agent execution in your platform
Key takeaway: Replace a single massive context window with a stateless loop that persists progress in git commits and picks up fresh tasks each iteration — steering via STEERING.md without restarting.

### 12. Programmatic Tool Calling (PTC)
📁 `programmatic-tool-calling/` | Why: Anthropic's own architecture for eliminating the per-call round-trip tax in multi-step tool workflows
Key takeaway: PTC lets Claude write code that orchestrates tool calls inside a container, achieving 11% better accuracy with 24% fewer tokens by keeping intermediate results out of context.

### 13. Harness Engineering: Leveraging Codex in an Agent-First World
📁 `openai-harness-engineering-blog/` | Why: The most concrete proof that control infrastructure (linters, CI, docs structure) — not code — is the moat
Key takeaway: 3 engineers shipped ~1M lines of 100% agent-generated code in 5 months by optimizing repo structure for agent legibility first, human readability second.

---

## 🟡 Worth Reading (25 articles)

### Score 4 — High value, read when you have time

**Cloudflare Code Mode MCP**
📁 `cloudflare-code-mode-mcp/` | Why: Solves MCP token bloat — collapses entire APIs into 2 tools at ~1,000 tokens
Key takeaway: `search()` + `execute()` in a V8 sandbox eliminates the need for predefined tool sets.

**Code Factory: Agent Auto Write and Review**
📁 `code-factory-agent-auto-write-review-code/` | Why: Production-grade CI/CD patterns for agent-generated code
Key takeaway: Risk-tiered policy gates with strict SHA matching and automated remediation loops that never bypass guardrails.

**Anthropic 2026 Agentic Coding Trends Report**
📁 `anthropic-2026-agentic-coding-trends-report/` | Why: Anthropic's own data on how the industry is shifting
Key takeaway: 27% of AI-assisted work consists of tasks that wouldn't have been done at all without AI — the gain is expanded output, not just speed.

**Context Rot: How Input Tokens Impact LLM Performance**
📁 `context-rot-increasing-input-tokens-llm-performance/` | Why: Research proving that context engineering is more important than context size
Key takeaway: Even a single distractor reduces accuracy; coherent haystacks hurt more than shuffled ones — how you present information matters as much as what you include.

**Claude + Codex Iterative Plan Review**
📁 `claude-codex-iterative-plan-review/` | Why: Actionable multi-model review pattern implementable as a Claude Code skill
Key takeaway: Pitting two models against each other caught 14 issues (auth, schema, concurrency) in 3 automated rounds — zero manual review.

**Agent Sandbox Infrastructure**
📁 `agent-sandbox-infrastructure/` | Why: Zero-secret sandbox architecture for code-executing agents
Key takeaway: Isolate the entire agent (not just tools) in a disposable micro-VM that talks to a credential-holding control plane.

**The 5 Levels of Agentic Software**
📁 `five-levels-of-agentic-software/` | Why: Prevents over-engineering — build incrementally from tools to storage to memory to multi-agent to production
Key takeaway: Most teams fail by jumping to multi-agent orchestration when a single well-instructed agent with good tools would have solved the problem.

**Pal: A Personal Context Agent**
📁 `pal-personal-context-agent-learns-how-you-work/` | Why: Open-source reference architecture for personal productivity agents
Key takeaway: A "context graph" (SQL + markdown + knowledge index + learnings) with a Classify→Recall→Retrieve→Act→Learn loop replaces RAG-style vector search.

**Claude Code Scheduled Tasks (Docs)**
📁 `claude-code-scheduled-tasks-docs/` | Why: Reference for the /loop and cron features you're already using
Key takeaway: Tasks are session-scoped (lost on exit), 50-task limit, 3-day expiry — use Desktop or GitHub Actions for persistence.

**8 Principles of Harness Engineering (Insights)**
📁 `openai-harness-engineering-insights/` | Why: Complements the OpenAI blog with practical principles
Key takeaway: Custom linter error messages are designed to inject remediation instructions directly into the agent's context.

**Agentic Software Engineering (Production)**
📁 `agentic-software-engineering/` | Why: Framework for taking agents from prototype to production
Key takeaway: Agent engineering = 40% agent dev + 40% system design + 20% security engineering. Six pillars: durability, isolation, governance, persistence, scale, composability.

**Everything is Context (arXiv paper)**
📁 `arxiv-2512-05470/` | Why: Academic foundation for the "everything is a file" context engineering approach
Key takeaway: Seven memory types (scratchpad, episodic, fact, experiential, procedural, user, historical) classified along temporal, structural, and representational dimensions.

### Score 3 — Useful context, partial applicability

**Vibe Coding 2.0: 18 Rules**
📁 `vibe-coding-2-18-rules-top-1-percent-builder/` | Good stack selection principles for MVPs.

**Just Talk To It — Agentic Engineering**
📁 `just-talk-to-it-agentic-engineering/` | Contrarian take: MCPs are "marketing theater" and CLIs beat them on context tax. Worth considering.

**Cowork Setup Guide**
📁 `cowork-setup-guide/` | Context files concept (about-me.md, brand-voice.md) is transferable to any agent setup.

**AI Agents 101 (Vasuman)**
📁 `ai-agents-101-vasuman-comprehensive-guide/` | Solid fundamentals if you need a refresher on agent architecture basics.

**Your Company Is a Filesystem**
📁 `your-company-is-a-filesystem/` | Reinforces the file-based state management pattern you're already using.

**Enforcing Design System Compliance**
📁 `enforcing-design-system-compliance/` | Applicable 5-layer enforcement pattern (docs → SKILL.md → lints → hooks → CI) for React Native design systems.

**Your LLM Writes Plausible Code, Not Correct Code**
📁 `llm-doesnt-write-correct-code-plausible/` | Important cautionary data: semantic bugs are the real danger, not syntax errors.

**AutoHarness: Automatic Harness Synthesis**
📁 `autoharness-automatic-harness-synthesis/` | Harness quality matters more than model size — smaller models with better scaffolding outperform larger ones.

**MCP: The Only Guide to Connect Claude to Everything**
📁 `mcp-only-guide-connect-claude-everything/` | Practical MCP setup reference (limited depth in summary).

**AI Engineer Full Course (hoeem)**
📁 `ai-engineer-full-course-hoeem/` | Three-layer framework (prompt → context → intent engineering) with 7-component intent schema.

**What Spec-Driven Development Gets Wrong**
📁 `what-spec-driven-development-gets-wrong/` | Key insight: specs must be bidirectional — agents write back findings as they discover reality diverges.

**How to Build a Data Agent in 2026**
📁 `how-to-build-a-data-agent-2026/` | Transferable patterns: "quirks" as learned corrections, self-scoring loops, context sub-agents.

**Harness Engineering Is Cybernetics**
📁 `harness-engineering-is-cybernetics/` | Good mental model — the engineer's role shifts from generating code to designing feedback loops.

---

## ⚪ Skim or Skip (19 articles)

**AI Doesn't Reduce Work — It Intensifies It** — `ai-doesnt-reduce-work-it-intensifies-it/` — HBR opinion on burnout; no actionable techniques.

**How to Set Up Claude the Right Way** — `how-to-set-up-claude-the-right-way/` — Shallow Claude (not Code) setup tips for beginners.

**AI Skills Are Not Durable** — `ai-skills-are-not-durable/` — Contrarian opinion that AI skills depreciate; thought-provoking but no techniques.

**Cowork Plugin Tier List** — `cowork-plugin-tier-list/` — Cowork plugin reviews; limited applicability to agent platform building.

**How We Rebuilt Next.js with AI (vinext)** — `cloudflare-vinext/` — Impressive case study but tangential to your stack.

**The Vibe Coding Trap** — `the-vibe-coding-trap/` — Build-vs-buy analysis; strategic insight but not actionable.

**Digital Gods: Death of Apps, Rise of the Meta-App** — `digital-gods-death-of-apps-rise-of-meta-app/` — Vision piece; no implementation guidance.

**10 Years Building Vertical Software: The Selloff** — `10-years-building-vertical-software-selloff/` — SaaS disruption analysis; industry awareness only.

**Annotated Screenshots for agent-browser** — `annotated-screenshots-agent-browser/` — Niche browser agent feature.

**Chrome Memory Snapshot + Cursor AI** — `chrome-memory-snapshot-cursor-ai-perf/` — Narrow debugging technique, Cursor-specific.

**Codex Security: Research Preview** — `openai-codex-security-research-preview/` — OpenAI product announcement; awareness only.

**The Rise of the Agent Builder** — `rise-of-the-agent-builder/` — Role prediction piece; no implementation guidance.

**Claude Cowork Setup Guide (Hassid)** — `claude-cowork-setup-guide-2/` — Beginner Cowork onboarding; covered better elsewhere.

**SWE-CI: Alibaba's AI Code Maintenance Benchmark** — `alibaba-swe-ci-ai-code-maintenance/` — Research benchmark; confirms Claude Opus 4 is best but no actionable techniques.

**Claude Cowork Scheduled Tasks for Non-Techies** — `claude-cowork-scheduled-tasks-non-techies/` — Non-technical audience; nothing beyond the docs version.

**GPT-5.4 Thinking and Pro Rolling Out** — `openai-gpt54-thinking-pro-rollout/` — Product announcement with no actionable content.

**Skills Worth $500/Hour in 2027** — `skills-worth-500-per-hour-in-2027-free-to-learn/` — Motivational career advice; zero technical content.

**Grep Is Dead (QMD Recall) — Duplicate** — `grep-is-dead-claude-code-qmd-recall/` — Same article as the Must Read version; shorter summary.

**Programmatic Tool Calling — Duplicate** — `programmatic-tool-calling-claude/` — Same topic as the Must Read version; shorter summary.

---

*Generated 2026-03-08. 57 articles scored across 5 dimensions. 2 duplicates identified and consolidated.*
