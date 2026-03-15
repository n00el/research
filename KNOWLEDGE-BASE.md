# Agentic Engineering Knowledge Base

> Synthesized from 30+ research articles. Actionable patterns for daily reference.
> Each insight cites its source as (folder-name).

---

## Claude Code Mastery

### Context Window Management

The #1 skill for productive agentic coding. Every file read, message, and command fills a limited "whiteboard" — performance degrades as it fills.

- **Use `/clear` aggressively** between unrelated tasks. Use `/compact` when context is large but you need continuity. (claude-code-best-practices-100x-powerful)
- **Subagents protect your context** — delegate research, searches, and exploratory work to subagents so intermediate results don't bloat the parent session. (lessons-claude-code-seeing-like-an-agent)
- **One fresh session per task contract** beats long-running sessions that accumulate context bloat. (how-to-be-world-class-agentic-engineer)
- **Separate research from implementation** — use a clean-context agent for research, then a fresh one for coding with only the findings. (how-to-be-world-class-agentic-engineer)
- **Context compaction at 60% loses half your decisions** — the memory problem is structural. Persist important state in files, not conversation history. (grep-is-dead-claude-code-remember-things)
- **Programmatic Tool Calling (PTC)** lets Claude orchestrate tool calls in code inside a container. Intermediate results stay in the code, not the context window. On benchmarks: **+11% accuracy, -24% tokens**. (programmatic-tool-calling)
- **Lazy-load context** — agents perform better pulling info on demand from an index than being frontloaded with everything. (simple-framework-agentic-systems)
- **Context rot is real** — models don't process context uniformly. Performance degrades non-linearly with length. Position matters: early items recalled better. Structure matters: shuffled content hurts more than structured. (context-rot-increasing-input-tokens-llm-performance)

**Common context failure patterns** (claude-code-best-practices-100x-powerful):
- **Kitchen sink session**: drifted through tasks A→B→C→D. Fix: `/clear` between unrelated tasks
- **Correction loop**: corrected Claude twice, still wrong. Fix: after 2 fails, clear and write better starting prompt
- **Infinite exploration**: Claude reads hundreds of files, context gone before building. Fix: narrow scope OR use subagents

**Prompt caching architecture** (lessons-from-claude-code-prompt-caching):
```
Order content by stability (static-first, dynamic-last):
1. System prompts & tools (globally cached)
2. Project context (CLAUDE.md, rules)
3. Session data
4. Conversation history (most dynamic)
```
- Never modify the system prompt mid-conversation — inject updates as system-reminder messages
- Never switch models mid-conversation (breaks cache) — use subagents for model transitions
- Keep tool sets stable — use `defer_loading` stubs instead of adding/removing tools
- Prompt caching cuts long Opus sessions from **$50-100 → $10-19**
- Treat cache hit rate as infrastructure metric with alerting

### CLAUDE.md Configuration

CLAUDE.md is injected into the system prompt — Claude follows it more strictly than chat messages. (claudemd-masterclass-from-start-to-pro)

**Hard budget: ~100-150 usable instruction slots.** Claude Code's system prompt uses ~50, leaving you 100-150 before instruction-following degrades uniformly across all rules.

**Do:**
- Keep it lean — only universally applicable rules
- Use progressive disclosure: offload task-specific rules to separate files referenced by path
- Use the 3-level hierarchy: global (`~/.claude/CLAUDE.md`), project root, nested directories
- Update with your PRs — treat it as a living document
- Make it a minimal conditional directory: `"if X, read this file"` (how-to-be-world-class-agentic-engineer)
- Pair with hooks (enforcement) and subagents (execution): rules define → hooks enforce → subagents execute

**Don't:**
- Use it as a linter — enforce code style through ESLint/Black/Ruff + pre-commit hooks instead of burning instruction budget
- Write monolithic docs — AGENTS.md/CLAUDE.md should be a short table of contents linking to layered docs/ (openai-harness-engineering-blog)
- Add formatting rules — they waste instruction slots on what tooling should handle

**Template pattern:**
```markdown
# Project: MyApp

## Architecture
- Monorepo: apps/ packages/ shared/
- If working in apps/api → read docs/api-conventions.md
- If working in apps/web → read docs/frontend-guide.md

## Rules
- All new endpoints need integration tests
- Use Zod for validation at system boundaries
- Never commit .env files

## Testing
- Run `npm test -- --related` before completing any task
```

### Skills & Slash Commands

Skills are reusable instruction packages that turn Claude into a domain expert. (complete-guide-building-skills-for-claude)

**Three-level progressive disclosure:**
1. **YAML frontmatter** — description + triggers (always loaded, controls when skill activates)
2. **SKILL.md body** — full instructions (loaded only when triggered)
3. **Linked files** — deep reference material (loaded on demand)

**The description field is everything** — it controls when Claude loads the skill:
```yaml
---
description: |
  Deploy the application to staging or production.
  Use when: "deploy", "ship it", "push to staging",
  "release to prod", "publish the app"
---
```
- Vague descriptions ("helps with projects") → undertriggering
- Overly broad descriptions → overtriggering
- Include specific trigger phrases users would actually say

**Three skill types:**
1. **Document creation** — standalone output, no tools needed
2. **Workflow automation** — multi-step with validation gates
3. **MCP enhancement** — workflow guidance on top of MCP tool access

**Key practices:**
- For critical validations, bundle deterministic scripts — code is deterministic, language is not
- Perfect a single challenging task first, extract the winning approach, then expand coverage
- Skill graphs (interconnected files with wikilinks) scale better than monolithic SKILL.md files (skill-graphs-greater-than-skill-md)
- Folder naming: **kebab-case only** (`notion-project-setup`), no spaces/underscores/capitals
- No `README.md` inside skill folders — all docs go in SKILL.md or references/
- No XML angle brackets in frontmatter (security: frontmatter appears in system prompt)
- Two skill loading modes: **reference** (`user-invocable: false`, auto-loaded) vs **task** (slash commands, user-triggered) (file-system-personal-os-for-ai-agents)

### Parallel Workflows & Worktrees

Run 3-5 parallel Claude sessions using git worktrees. (claude-code-best-practices-100x-powerful)

```bash
# Create worktrees for parallel work
git worktree add ../myproject-feat-auth -b feat/auth
git worktree add ../myproject-feat-api -b feat/api
git worktree add ../myproject-fix-tests -b fix/tests

# Run Claude in each worktree independently
# They share the same repo but don't interfere
```

**Patterns:**
- One session writes code, another writes tests, a third reviews
- Use Plan Mode first: read/map codebase → produce plan → review → execute (10 min upfront prevents hours of rework)
- **"Blast radius" thinking**: estimate how many files a change touches and how long it takes. Stop and reassess when things exceed estimates. (just-talk-to-it-agentic-engineering)
- Dedicate ~20% of time to refactoring agent-generated code with tools like jscpd, knip, eslint plugins (just-talk-to-it-agentic-engineering)

---

## Agentic Engineering

### Agent Architecture Patterns

**The 5 levels of agentic software** — build incrementally, each level justified by failure of the simpler approach (five-levels-of-agentic-software):

| Level | What | When to add |
|-------|------|-------------|
| 1. Tools | LLM + read/write/execute | Always start here |
| 2. Knowledge | Agentic RAG over specs, docs, decision records | When LLM training data isn't enough |
| 3. Memory | Store learnings, build user profiles across sessions | When agent serves same users repeatedly |
| 4. Multi-agent | Specialized agents coordinated by orchestrator | When single agent demonstrably fails |
| 5. Production | PostgreSQL, PgVector, tracing, observability | When shipping to real users |

> Most teams fail by jumping to Level 4 when Level 1-2 would suffice.

**Production agent engineering = 40% agent dev + 40% system design + 20% security** (agentic-software-engineering)

**Six production pillars:** Durability, Isolation, Governance, Persistence, Scale, Composability

**Tool design principles** (lessons-claude-code-seeing-like-an-agent):
- Design tools to match the model's actual abilities, not theoretical use cases
- Keep tool count low (~20 for Claude Code) — each tool increases decision space
- Revisit tool design with every model upgrade — previously essential tools can become constraints
- Agent-driven search (Grep) beats RAG — models build their own context better than pre-indexed results
- Minimize action space — too many tools causes "agentic dementia" (decision paralysis) (simple-framework-agentic-systems)

**Promote an action to a tool only for these 5 reasons** (programmatic-tool-calling):
1. UX rendering needs
2. Safety guardrails
3. Concurrency control
4. Observability
5. Autonomy level control

### Multi-Agent Coordination

**Orchestrator pattern** (anthropic-2026-agentic-coding-trends-report):
- Central orchestrator coordinates specialized parallel agents
- Each agent has focused context and tools
- Replacing single-agent workflows for complex tasks

**Adversarial multi-agent review** (claude-codex-iterative-plan-review):
```
Plan → Review → Revise → Re-review (repeat until APPROVED)
```
- A second model as adversarial reviewer catches blind spots the planner misses
- In real-world test: 3 rounds caught 14 issues (broken auth, schema conflicts, missing concurrency)
- Reserve for high-stakes plans (auth, data models, multi-day implementations)

**Bug-finder + Disprover + Referee** pattern — exploit model sycophancy by having agents challenge each other (how-to-be-world-class-agentic-engineer)

**Key insight:** prefer explicit workflows over dynamic delegation for production reliability. Reserve dynamic teams for human-supervised settings. (five-levels-of-agentic-software)

**When to use subagents/multi-agent** (simple-framework-agentic-systems):
- Diverse workloads needing different tools
- Long tool outputs that need distillation
- Very long tasks exceeding context limits
- Parallelizable independent work

### Harness Engineering

The engineer's role shifts from writing code to designing the environment agents operate in. (harness-engineering-is-cybernetics)

**OpenAI's proof point:** 3 engineers shipped ~1M lines of 100% agent-generated code in 5 months via ~1,500 PRs (~3.5 PRs/engineer/day). (openai-harness-engineering-blog)

**8 Principles** (openai-harness-engineering-insights):
1. Progressive disclosure in docs (concise index → layered reference)
2. Mechanical enforcement via linters/CI, not natural-language instructions
3. Strict layered architecture (Types→Config→Repo→Service→Runtime→UI) enforced by structural tests
4. Custom linter error messages that inject remediation instructions into agent context
5. Repository optimized for agent legibility first, human readability second
6. Agent-to-agent code review (humans only for edge cases)
7. Background agents continuously scan for deviations
8. Velocity-first merge: address test failures post-merge rather than blocking

**When agents produce bad output, the problem is almost never capability — it's that domain knowledge hasn't been externalized into machine-readable form.** Fix: architecture docs, custom linters with remediation instructions, golden principles, codified standards. (harness-engineering-is-cybernetics)

**AutoHarness insight:** harness quality matters more than model size. Auto-synthesized scaffolding let Gemini-2.5-Flash beat Gemini-2.5-Pro and GPT-5.2-High while reducing costs. (autoharness-automatic-harness-synthesis)

**Code Factory pattern** (code-factory-agent-auto-write-review-code):
```json
{
  "risk_tiers": {
    "high": { "paths": ["src/auth/**", "src/payments/**"], "checks": ["unit", "integration", "security-scan"] },
    "medium": { "paths": ["src/api/**"], "checks": ["unit", "integration"] },
    "low": { "paths": ["src/utils/**", "docs/**"], "checks": ["unit"] }
  }
}
```
- Run risk-policy gate before expensive CI
- Enforce current-head SHA matching — never trust stale "clean" evidence
- Automated remediation loop: agent reads findings → patches → pushes, but never bypasses gates

**Ralph Loop for long-running agents** (ralph-loop-setup-long-running-ai-agents):
- Each iteration gets fresh context — state persists in git commits, not conversation
- Runs in Docker sandbox, 30+ hours unattended
- Mid-run steering via STEERING.md (agent reads at start of each iteration)
- Every completed task = git commit → rollback is trivial
- Best for: prototyping, MVPs, test generation, migrations, bulk refactoring
- Struggles with: pixel-perfect design, novel architecture, security-critical code

---

## MCP & Tool Integration

### Server Setup

MCP (Model Context Protocol) connects Claude to external tools, databases, and APIs via a standardized open protocol. (mcp-only-guide-connect-claude-everything)

**Key consideration:** MCPs carry context tax. GitHub MCP = 23k tokens vs `gh` CLI = 0 tokens. Use CLIs when the model already knows them. (just-talk-to-it-agentic-engineering)

**When MCP wins over CLI:**
- The tool doesn't have a good CLI
- You need structured input/output for the model
- The integration requires auth state management
- You want to add workflow guidance via skills on top

### Protocol Patterns

**Code Mode — the token-efficient pattern** (cloudflare-code-mode-mcp):

Cloudflare's 2,500+ endpoints would consume ~1.17M tokens as individual MCP tools. Code Mode collapses them into 2 tools at ~1,000 tokens:

```
search() → agent discovers endpoints by querying OpenAPI spec
execute() → agent writes JS that runs in sandboxed V8 isolate
```

- **99.9% token reduction** vs tool-per-endpoint
- New endpoints auto-discoverable without registering new tools
- Sandboxed execution: no filesystem, no env vars, no external fetch by default
- Open-sourced in Cloudflare Agents SDK

**Tool design rules:**
- Name tools well, describe them correctly, add usage examples
- The best tool is useless if the model doesn't understand how to call it
- Keep tool count minimal — each one increases the decision space

---

## AI-Assisted Development

### Prompt Craft for Code

**Three layers of AI engineering** (ai-engineer-full-course-hoeem):
1. **Prompt engineering** (foundation) — structural formatting, few-shot, chain-of-thought
2. **Context engineering** (multiplier) — context auditing, building context files, RAG design
3. **Intent engineering** (strategic) — objective, outcomes, health metrics, constraints, delegation protocol, stop rules

> Prompt engineering is the least important of the three. Context and intent matter more.

**Always give verifiable acceptance criteria** — test cases, not descriptions. Agents struggle knowing when they're "done" without deterministic checks. (claude-code-best-practices-100x-powerful)

**Task contracts** (how-to-be-world-class-agentic-engineer):
```markdown
## Task: Add user avatar upload
### Acceptance criteria
- [ ] POST /api/avatar accepts image/png, image/jpeg < 5MB
- [ ] Returns 400 for invalid files with descriptive error
- [ ] Stores in S3 with key users/{id}/avatar.{ext}
- [ ] `npm test -- --grep avatar` passes
### Context
- Read src/api/routes/user.ts for route patterns
- Read src/services/s3.ts for storage patterns
```

**System prompt design** (simple-framework-agentic-systems):
- Explicitly define: observations (what agent sees), expected outcomes (success criteria), available tools
- Not just a role description — be specific about inputs and outputs

**Voice dictation produces ~3x better prompts** — talking naturally adds more detail, background, and constraints than typing. Mac: double-tap Function key. (claude-code-best-practices-100x-powerful)

**Self-maintaining specs** beat static documentation. Both humans and agents read/write to the spec. Agents surface decisions that change direction (like a good junior engineer), humans approve before execution. Every documentation-first initiative fails because nobody rewards continuous maintenance. (what-spec-driven-development-gets-wrong)

### Code Review Patterns

**AI-assisted quality assurance is becoming standard** (anthropic-2026-agentic-coding-trends-report):
- Organizations use AI agents to review large-scale AI-generated output
- Analyzing for security vulnerabilities, architectural consistency, quality issues
- Agent-to-agent review with humans only for edge cases

**SWE-CI finding:** 75% of AI models break previously working code during maintenance. Only Claude Opus 4 maintains above 50% zero-regression rate. (alibaba-swe-ci-ai-code-maintenance)

**Practical patterns:**
- Cloudflare rebuilt Next.js API with 800+ Claude sessions (~$1,100): task → AI writes implementation + tests → run suite → merge if pass → iterate if not → AI reviews PRs → AI addresses review comments (cloudflare-vinext)
- Convert production regressions into harness-gap issues with SLA tracking so fixes grow long-term coverage (code-factory-agent-auto-write-review-code)
- Background agents continuously scan for deviations and open targeted refactoring PRs (openai-harness-engineering-blog)

### Vibe Coding (Do's and Don'ts)

**The trap:** AI makes building software nearly free but owning it (maintenance, compliance, ops) stays expensive. Building ≠ owning. (the-vibe-coding-trap)

**Do:**
- Make architectural decisions before writing any code (vibe-coding-2-18-rules-top-1-percent-builder)
- Use managed services for auth (Clerk), realtime (Supabase), deploy (Vercel) — never build these from scratch for an MVP
- Adopt an opinionated stack early (shadcn/ui, Tailwind, Zustand, tRPC, Prisma, Zod) to eliminate decision fatigue
- Set up error tracking (Sentry) and analytics (PostHog) from day one
- Use version control with feature branches — AI agents make confident mistakes at speed
- Schedule tech debt cleanup after every 2-3 features
- Ship imperfect MVPs fast and iterate — perfectionism kills velocity
- Document architectural decisions for future contributors

**Don't:**
- Accept AI output without understanding it — you own every bug, compliance issue, and operational burden
- Skip version control because "it's just a prototype"
- Build auth, payments, or deployment pipelines from scratch for an MVP
- Fly blind in production without error tracking
- Assume building = owning (the ownership tax: maintenance, compliance, ops)

**The real skill:** not writing better code, but ruthlessly identifying what NOT to build.

---

## Personal AI Agents

### Memory & Context

**The structural problem:** Claude Code sessions start from zero. Context compaction loses decisions. Memory must be externalized. (grep-is-dead-claude-code-remember-things)

**QMD approach:**
- Index Obsidian vault with BM25 + semantic search
- BM25 handles 80% of searches (fast, deterministic)
- Semantic search fills the gap for fuzzy/conceptual queries
- Post-session hook auto-exports conversations to the index
- `/recall` skill with three modes: temporal, topic, graph

**File-system as personal OS** (file-system-personal-os-for-ai-agents):
```
brain/
  AGENT.md          # Core rules + decision table
  identity.yaml     # Voice, style as quantitative data (7/10 scales)
  episodic/         # Decisions with reasoning, failures with root causes
  skills/
    reference/      # Auto-loaded silently for consistency
    task/           # Manually invoked via slash commands
modules/
  brand/            # Separate from identity (saves 40% tokens on voice-only tasks)
  clients/          # Per-client context
  projects/         # Per-project state
```

**File format rules:**
- **JSONL** for append-only logs (agents can overwrite JSON — use JSONL to prevent data loss)
- **YAML** for hierarchical config
- **Markdown** for narrative content LLMs read natively

**Pal context agent pattern** (pal-personal-context-agent-learns-how-you-work):
- Context graph with 4 layers: SQL database, markdown config, knowledge index, learned strategies
- 5-step loop: Classify → Recall → Retrieve → Act → Learn
- Agent manages its own DB schema and evolves it as data grows
- Key: prompt stays the same but agent improves over time as structured memory grows
- Governance at the "external impact" boundary: read freely, gate actions affecting others

**Episodic memory** — store not just facts but judgment (file-system-personal-os-for-ai-agents):
- `experiences.jsonl` — key moments with emotional weight scores (1-10)
- `decisions.jsonl` — key decisions with reasoning, alternatives considered, outcomes tracked
- `failures.jsonl` — what went wrong, root cause, prevention steps
- Facts tell what happened. Episodic memory tells what mattered and how you think about tradeoffs.

**Progressive disclosure** beats monolithic system prompts — load only the context needed for each task. Module boundaries = loading decisions. (file-system-personal-os-for-ai-agents)

### Scheduling & Automation

**Claude Code `/loop` command** (claude-code-scheduled-tasks-docs):
```
/loop 10m /review-pr 1234     # Check PR every 10 minutes
/loop 30m check build status   # Monitor builds
```
- Session-scoped only — tasks lost when Claude Code exits
- 50-task limit, 3-day auto-expiry on recurring tasks
- For persistent automation: use Desktop scheduled tasks or GitHub Actions with `schedule` trigger
- One-time reminders: natural language ("remind me at 3pm to...")
- All times use local timezone

**Cowork scheduled tasks for non-technical users** (claude-cowork-scheduled-tasks-non-techies):
- Plain English instructions + schedule + output folder
- Tasks survive sleep mode but die if you close the app

**Ralph Loop for persistent automation** (ralph-loop-setup-long-running-ai-agents):
- PRD with discrete, verifiable tasks → Docker sandbox → fresh context per iteration
- State in git commits, steering via STEERING.md
- Developer role: planning, delegating, reviewing — not typing code

**Background agent patterns** (anthropic-2026-agentic-coding-trends-report):
- Long-running agents extending from minutes to multi-day sessions
- Scheduled background tasks (daily briefings, inbox digests) turn reactive tools into compounding systems (pal-personal-context-agent-learns-how-you-work)
- ~27% of AI-assisted work is tasks that wouldn't have been done at all without AI

---

## Quick Reference Card

| Problem | Solution | Source |
|---------|----------|--------|
| Context window filling up | `/clear`, `/compact`, subagents | best-practices-100x |
| CLAUDE.md too long | Progressive disclosure, conditional includes | claudemd-masterclass |
| Agent produces bad output | Externalize domain knowledge into docs/linters | harness-cybernetics |
| Need parallel work | Git worktrees, 3-5 sessions | best-practices-100x |
| Plan quality uncertain | Adversarial multi-model review loop | codex-iterative-plan |
| Large API as MCP | Code Mode (search + execute, ~1k tokens) | cloudflare-code-mode |
| Long-running task | Ralph Loop (fresh context + git commits) | ralph-loop |
| Agent doesn't know when done | Deterministic test-based acceptance criteria | world-class-engineer |
| Memory between sessions | QMD index + /recall skill | grep-is-dead |
| Recurring automation | `/loop` (session) or GitHub Actions (persistent) | scheduled-tasks-docs |
| Choosing agent complexity | Start Level 1, add only when current level fails | five-levels |
| Prompt caching costs | Static-first ordering, never mutate system prompt | prompt-caching |
| Vague prompts → bad code | Use voice dictation (~3x better) or task contracts | best-practices-100x |
| Context degrades with length | Place critical info early, keep structure intact | context-rot |
| Specs go stale | Self-maintaining specs (both human + agent write) | spec-driven-dev |
| Correction loop (2+ fails) | Clear session, write better starting prompt | best-practices-100x |
| Agent breaks existing code | 75% of models regress; use tests as gates | alibaba-swe-ci |
