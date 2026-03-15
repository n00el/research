# How To Be A World-Class Agentic Engineer

**Author:** sysls (@systematicls) — builder of @openforage, ex-hedge fund systematic investment
**Date:** March 3, 2026
**Source:** https://x.com/systematicls/status/2028814227004395561
**Stats:** 7,377 likes, 860 retweets, 159 replies, 23,226 bookmarks, 2.7M views

---

A practical guide for developers using Claude Code and Codex CLI on how to stop over-engineering their agentic workflows and instead master a few core principles. The central thesis: **you do not need complex harnesses, packages, or plugins -- a barebones CLI setup combined with a handful of key principles will outperform over-tooled setups.**

## 1. Keep It Simple — The World Is Sprinting By

- Every new generation of AI agents changes what is optimal, so locking yourself into external libraries and harnesses is counterproductive.
- If a solution truly solves a real pain point, the frontier companies (Anthropic, OpenAI) will incorporate it into their products. Skills, memory, subagents, planning -- all started as external solutions and were absorbed into the base products.
- Just update your CLI tool periodically and read the release notes.

> "You don't need the latest agentic harnesses, you don't need to install a million packages and you absolutely do not need to feel the need to read a million things to stay competitive."

## 2. Context Is Everything

- Agents perform best when given exactly the information needed for their task, and nothing more.
- External plugins, memory systems, and poorly named skills cause "context bloat" -- overwhelming the agent with irrelevant information, degrading performance.

> "You want to give your agents only the exact amount of information they need to do their tasks and nothing more!"

## 3. Be Precise About Implementation

- Separate research from implementation. Do not ask vague questions like "build an auth system."
- Specify exact requirements: "implement JWT authentication with bcrypt-12 password hashing, refresh token rotation with 7-day expiry."
- If you lack details, create a separate research task first, decide on an approach, then hand off to a fresh agent with clean context.

## 4. Exploit Sycophancy Intentionally

- Agents are designed to please. If you say "find a bug," they will find one -- even if they have to invent it.
- Use "neutral" prompts that do not bias the agent toward a predetermined outcome.
- **Multi-agent adversarial pattern:** a bug-finder agent (overreports), an adversarial agent (tries to disprove bugs), and a referee agent (judges both). This exploits each agent's desire to please in opposing directions for high-fidelity results.

> "If you ask for something, it will deliver -- even if it has to stretch the truth a little!"

## 5. Compaction, Context, and Assumptions

- Agents are still poor at "connecting the dots" or making assumptions. When they do, quality drops sharply.
- After context compaction, instruct agents to re-read their task plan and relevant files before continuing.

## 6. Let Agents Know How To End a Task

- Agents know how to start tasks but struggle with knowing when they are done, often producing stubs.
- Use deterministic tests as milestones: "unless these N tests pass, your task is NOT complete."
- Screenshots plus verification is another viable endpoint.
- Create a `{TASK}_CONTRACT.md` specifying tests, screenshots, and verification required before the session can terminate.

## 7. Agents That Run Forever — Don't

- Long-running 24-hour sessions are suboptimal because they accumulate context bloat from unrelated contracts.
- Better approach: one new session per contract, with an orchestration layer creating contracts and spawning fresh sessions.

## 8. Iterate: Rules and Skills

- Treat `CLAUDE.md` as a minimal, logical directory of conditional pointers: "if doing X, read this file; if doing Y, read that file."
- **Rules** encode preferences (things you do not want the agent to do).
- **Skills** encode recipes (specific ways you want tasks done).
- As rules and skills accumulate, they start contradicting each other. Periodically consolidate -- have the agent clean up contradictions and redundancies.

> "Treat your CLAUDE.md as a logical, nested directory of where to find context given a scenario and an outcome. It should be as barebones as possible."

## 9. Own The Outcome

No agent is perfect. You can delegate design and implementation, but you must own and verify the final result.

> "That's it. That's really the secret. Keep it simple, use rules and skills and CLAUDE.md as a directory and be religiously mindful about their context and their design limitations."
