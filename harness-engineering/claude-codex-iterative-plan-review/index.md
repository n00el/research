# I Made Claude and Codex Argue Until My Code Plan Was Actually Good

**Author:** Aseem Shrey
**Date:** February 20, 2026
**Source:** https://aseemshrey.in/blog/claude-codex-iterative-plan-review/
**Tags:** AI, Developer Tools, Claude Code, Codex

---

A Claude Code skill that pits Claude against Codex in an iterative review loop. 3 rounds, 14 issues caught, zero manual effort.

### TL;DR

A [Claude Code skill](https://docs.anthropic.com/en/docs/claude-code/skills) that sends your implementation plans to OpenAI Codex for review. They argue back and forth -- Claude revises, Codex re-reviews -- until Codex approves. Typically converges in **2-3 rounds**.

**Get it** -> [SKILL.md on GitHub Gist](https://gist.github.com/LuD1161/84102959a9375961ad9252e4d16ed592)

**Setup:**
1. Install [Codex CLI](https://github.com/openai/codex): `npm install -g @openai/codex`
2. Drop `SKILL.md` into `.claude/skills/codex-review/` in your project
3. Run `/codex-review` in Claude Code when you have a plan ready

**Use it for:** Plans touching auth, data models, concurrency, or anything that will take days to implement.
**Skip it for:** Simple bug fixes, small changes, or when speed matters more than thoroughness.

**Result:** 3 rounds caught **14 issues** (broken auth, shell quoting bugs, schema conflicts, missing concurrency handling) and turned a rough draft into a production-grade spec -- zero manual review effort.

---

I use Claude Code daily. It plans features, writes code, and ships PRs. But I kept running into the same problem: **Claude's plans were good, but not challenged.** There was no second opinion. No one pushing back on architectural blind spots, missing edge cases, or security gaps.

So I built a system where **Codex reviews Claude's plans** -- and they go back-and-forth until Codex approves.

## The Problem: Single-Model Blindness

When you use a single AI model to plan and execute, you get a coherent but unchallenged output. The model doesn't argue with itself. It won't say "actually, this auth model is incomplete" or "your shell quoting is broken."

I noticed this pattern repeatedly:

- Plans that looked solid but had no authentication model
- Shell scripts with quoting bugs that would silently produce bad payloads
- Schema designs with conflicting state fields
- No concurrency handling for multi-agent scenarios

These aren't bugs Claude can't find -- it's that the planner and the reviewer are the same entity. There's no adversarial tension.

## The Solution: `/codex-review`

I built a [Claude Code skill](https://docs.anthropic.com/en/docs/claude-code/skills) -- a slash command that triggers an iterative review loop between Claude and OpenAI's Codex CLI.

### How it works

```
You: /codex-review

Round 1: Claude writes plan to temp file
  -> sends to Codex (gpt-5.3-codex, read-only sandbox)
  -> Codex reviews
  -> VERDICT: REVISE (8 issues found)

Round 2: Claude revises plan addressing all feedback
  -> resumes Codex session (preserves context)
  -> Codex re-reviews
  -> VERDICT: REVISE (6 items left)

Round 3: Claude revises again
  -> resumes Codex session
  -> Codex
  -> VERDICT: APPROVED
```

The key insight: **Codex runs in read-only mode** -- it can read your codebase for context but can't modify anything. And because we **resume the Codex session** between rounds, it remembers what it said before and can verify whether issues were actually fixed.

## Before and After

### Before: Single-pass planning

I'd ask Claude to plan a feature. Claude would produce a plan. I'd read it, maybe catch a few things, approve it. But I'm not going to catch every quoting bug in a shell script example or notice that a `status` and `column` field in a schema can drift.

**Example**: I asked Claude to plan a "Mission Control" dashboard for a multi-agent swarm. The initial plan had:

- No authentication on agent write endpoints
- Shell scripts with `"$1"` inside single quotes (broken expansion)
- Both `status` and `column` fields on tasks (can drift)
- Unbounded embedded comment arrays (write contention)
- No concurrency handling for task claiming
- Manual-only testing

All of these are real problems that would have burned hours during implementation.

### After: Iterative cross-model review

Same plan, but run through `/codex-review`:

**Round 1** -- Codex found 8 issues across auth, schema design, tooling, and testing. It cited specific line numbers and gave actionable fixes.

**Round 2** -- After Claude revised, Codex found 6 remaining items: the lease claim wasn't atomic, the ACL was too broad, key rotation was underspecified, and the status model was inconsistent.

**Round 3** -- All addressed. Codex approved. The final plan had:

- Per-agent API key auth with server-side identity derivation
- A typed CLI tool instead of fragile shell scripts
- Atomic lease claims with optimistic concurrency
- An explicit ACL permission matrix
- Full key rotation lifecycle with grace periods
- Two-phase search strategy with compound indexes
- Comprehensive integration and security tests

**3 automated rounds. Zero manual review effort on my part.** The plan went from "demo quality" to "production-grade spec."

```
+------------------------------------------------------+
|                  Before vs After                      |
+----------------------+-------------------------------+
| BEFORE               | AFTER                         |
| Single-pass plan     | 3-round iterative review      |
+----------------------+-------------------------------+
| No auth model        | Per-agent API keys + ACL      |
| Broken shell scripts | Typed CLI with retries        |
| Conflicting schema   | Single source of truth        |
| No concurrency       | Atomic claims + versioning    |
| Manual testing only  | Integration + security tests  |
| 0 issues caught      | 14 issues caught & fixed      |
+----------------------+-------------------------------+
```

## Design Decisions

### Why a skill, not a hook?

I don't want every plan reviewed. Most small changes don't need a second opinion. A skill (slash command) is **on-demand** -- I trigger it only when the stakes are high enough.

### Why iterative, not one-shot?

A one-shot review finds problems but doesn't verify fixes. The iterative loop means:

- Codex finds issues
- Claude fixes them
- Codex verifies the fixes actually work
- Repeat until clean

This catches the "fixed one thing but broke another" class of problems.

### Why session resume?

Codex CLI supports `codex exec resume <session-id>`, which continues a previous conversation with full context. This means Codex remembers what it said in Round 1 when reviewing Round 2. It doesn't re-discover the same issues -- it checks whether they were addressed.

### Why the VERDICT protocol?

The loop needs to know when to stop. Codex ends each review with either `VERDICT: APPROVED` or `VERDICT: REVISE`. This gives Claude a clear signal to either continue revising or present the final result. Max 5 rounds as a safety cap.

### Concurrency safety

Each review generates a UUID for temp files (`/tmp/claude-plan-<uuid>.md`) and tracks the Codex session by its explicit ID. Multiple Claude Code instances can run `/codex-review` simultaneously without stepping on each other.

## Technical Implementation

The entire thing is a single Markdown file at `.claude/skills/codex-review/SKILL.md`. No external services, no infrastructure. It's instructions that tell Claude how to:

- Write the plan to a temp file
- Run `codex exec -m gpt-5.3-codex -s read-only` with a review prompt
- Parse the verdict
- If REVISE: update the plan, run `codex exec resume <session-id>`
- Loop until APPROVED or max 5 rounds
- Clean up temp files

The skill leverages what's already there -- Claude Code's tool execution and Codex CLI's session management.

## Gotchas I Found

- **`codex exec resume` doesn't support `-o`** -- You can't redirect output to a file on resume. Capture stdout instead.
- **`--last` is not concurrency-safe** -- Always use the explicit session ID, not `--last`, to resume. Otherwise parallel reviews grab the wrong session.
- **Codex needs explicit verdict instructions** -- Without the `VERDICT: APPROVED/REVISE` instruction, Codex gives nuanced feedback but no clear signal for the loop.

## When to Use This

- Planning a feature that touches auth, data models, or multi-service coordination
- Before approving a plan that will take days to implement
- When you want security review before writing code
- When the plan involves concurrency, distributed systems, or data migrations

When NOT to use it:

- Simple bug fixes or small changes
- Plans where you've already validated the approach
- When speed matters more than thoroughness

## What's Next

- **Model choice per review type** -- security reviews with one model, architecture reviews with another
- **Review templates** -- different review prompts for different plan types (API design vs. frontend vs. infrastructure)
- **Review history** -- persisting review results so you can reference past decisions

But honestly, the simple version already catches more issues than I expected. Three rounds of back-and-forth between two frontier models produces plans I actually trust.

---

*The skill is a single file you can drop into any Claude Code project. Grab it from the [GitHub Gist](https://gist.github.com/LuD1161/84102959a9375961ad9252e4d16ed592), drop it at `.claude/skills/codex-review/SKILL.md`, and run `/codex-review` next time you have a plan worth challenging.*

*P.S. -- Yes, this blog post was also written with Claude Code. I made an AI write about making AIs argue with each other. We're through the looking glass, people.*
