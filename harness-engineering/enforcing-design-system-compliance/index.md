# How to Force Your Agent to Obey Your Design System

**Author:** Ryan Carson (@ryancarson) — Dad, Dev, CEO, 4x Founder
**Date:** March 3, 2026
**Source:** https://x.com/ryancarson/status/2028916090596643078
**Stats:** 808 likes, 51 retweets, 2,369 bookmarks, 20 replies, 159,582 views

---

A deterministic, enforceable approach to making AI coding agents (such as Cursor, Amp, Claude Code, etc.) consistently follow a project's design system -- treating it as a **contract** rather than optional guidance.

> "Most design systems fail for one simple reason: they are optional. If you want reliable UI consistency, you need to stop treating your design system as guidance and start treating it like a contract."

## The 5-Layer Setup

### 1. Canonical Docs

A single source of truth for visual language and interaction rules. Design docs should be separated into at least three documents: design rules (hard DO/NEVER policies) and visual language (token mappings, component styling primitives, typography/color contracts).

### 2. Agent Routing (Skill/Instruction Layer)

A dedicated `design-system/SKILL.md` file that forces the agent to use the canonical docs when encountering design-related tasks. This turns "remember to check the design system" into deterministic behavior, preventing ad-hoc styling decisions.

### 3. Local Lint Rules (Custom Checks)

Off-the-shelf linters won't fully enforce a design system, so custom rules are needed:
- **Spacing token check**: Bans raw spacing utilities; requires semantic spacing tokens.
- **Color token check**: Bans raw hex/rgb/hsl literals and arbitrary color utilities.
- **Token contract check**: Ensures CSS vars, Tailwind tokens, and component primitives stay aligned.
- **Typography constant check**: Requires shared heading constants; disallows raw heading utilities.

### 4. Pre-commit Hooks

A single orchestrator command (`lint:design-system`) runs staged-file checks automatically before commits. One entrypoint is used so agents don't have to route through multiple overlapping commands.

### 5. CI Gates

CI must run the same `lint:design-system` contract and **fail the PR** if drift is introduced. Treating failures as blocking guarantees design consistency rather than merely encouraging it.

## Key Insights

- The approach enforces design compliance at **every stage** of the development pipeline: authoring (docs + agent routing), coding (lint rules), committing (pre-commit hooks), and merging (CI gates).
- Adds explicit **scope and exception policy** with clear rules about what is in scope and how to handle legitimate exceptions.
- Framework-agnostic and agent-agnostic -- works with any coding agent and any frontend stack.
