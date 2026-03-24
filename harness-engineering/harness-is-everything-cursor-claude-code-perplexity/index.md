# The Harness Is Everything: What Cursor, Claude Code, and Perplexity Actually Built

**Author:** Rohit (@rohit4verse)
**Date:** Mar 17, 2026
**Source:** https://x.com/rohit4verse/status/2033945654377283643
**Stats:** 77 replies, 269 reposts, 1,952 likes, 7,182 bookmarks, 960K views

---

![Header image](image-1.jpg)

You are not using AI wrong because you haven't found the right model.

You are using AI wrong because you haven't built the right environment.

There is a reason some teams are shipping a million lines of code with three engineers while others are struggling to get a consistent refactor out of their agent pipeline. The difference is not GPT-5 versus Claude Opus. The difference is not the temperature setting or the max tokens. It isn't even the prompt, though everyone loses months of their life arguing about prompts.

The difference is the harness.

This article is about what that word actually means, technically and philosophically, because the industry has developed a bad habit of using it loosely. A harness is not a system prompt. It is not a wrapper around an API call. It is not an eval framework or a prompt template or a chatbot with memory. A harness is the complete designed environment inside which a language model operates, including the tools it can call, the format of information it receives, how its history is compressed and managed, the guardrails that catch its mistakes before they cascade, and the scaffolding that allows it to hand off work to its future self without losing coherence.

When you look at what Anthropic built to make Claude Code actually work, what OpenAI built to ship a million lines of code through Codex with zero manually-written code, and what the Princeton NLP group published in their landmark SWE-agent paper about agent-computer interfaces, you start to see the same pattern emerging from every serious team working in this space.

The model is almost irrelevant. The harness is everything.

This is a detailed technical breakdown of how that idea became the defining insight of applied AI engineering in 2025 and 2026. It covers the research, the real implementations, the failure modes that motivated the design decisions, and the patterns that repeat whether you are building a coding agent, a research agent, or a long-running autonomous software engineer. By the end, you will understand not just what a harness is, but why building one correctly is now the most valuable engineering skill in the industry.

# Part One: The Problem Nobody Talks About

## Why Raw Capability Is Not Enough

In mid-2024, something strange happened in AI benchmarks. Researchers started noticing that the same frontier model could produce wildly different results on identical coding tasks depending entirely on how the task was presented and what tools were made available. The model had not changed. The underlying intelligence had not changed. What changed was the interface.

This should not have been surprising. We have known for decades that the right tools make engineers dramatically more productive. A software developer with a modern IDE, debugger, version control, and CI/CD pipeline is orders of magnitude more effective than the same developer working in a raw terminal with only a text editor. The IDE does not make the developer smarter. It removes friction, surfaces information at the right moment, catches errors early, and organizes work into navigable units.

Language models are the same. They are not general reasoners working from some infinite internal knowledge base. They are sophisticated pattern-matching engines that operate on tokens in a context window. Everything they know in a given moment is determined by what is in that context window, and everything they produce is conditioned on how that context is structured. The format of the input is not decoration. It is the cognitive architecture of the agent.

The interface is not a convenience layer. For an LM agent, the interface is the mind.

This is the central claim of the SWE-agent paper published by the Princeton NLP group in 2024, and it holds up under scrutiny. The paper introduced the concept of an Agent-Computer Interface (ACI) and demonstrated that a carefully designed ACI could produce a 64% relative improvement in benchmark performance compared to the same model interacting through a standard Linux shell. Same model, same task, same compute budget. The only variable was the interface.

Let that land for a moment. 64% is not a marginal gain. That is the difference between a tool that works and a tool that does not. And it came entirely from environment design, not from any improvement in the underlying model.

## The Context Window Is Not a RAM Slot

The naive mental model of an AI agent treats the context window like RAM. You load data in, the model processes it, you get output. More context equals better performance. Longer prompts equal richer understanding. This mental model is wrong in ways that will ruin your agent if you build around it.

The context window is actually closer to the agent's entire working consciousness for a given session. Every token in that window costs computation. Every irrelevant piece of information competes for attention with the relevant information. The model does not have a selective attention mechanism that cleanly ignores noise. The noise is in the room, and it affects the reasoning.

This has specific, measurable consequences for agent design. When you run grep on a large codebase from inside an agent loop and return ten thousand lines of matches, you have not given the agent more information to work with. You have flooded its working memory with irrelevant data that will degrade the quality of every subsequent step until the context is cleared. When you dump an entire file with cat because the agent wanted to see two functions, you have handed it a firehose when it needed a drinking glass.

The SWE-agent researchers were meticulous about documenting these failure modes. A standard bash interface caused agents to thrash. They would issue grep commands that returned thousands of lines, lose track of what they were looking for, issue more grep commands, gradually fill up their context with noise, and eventually either produce a wrong answer or stop making progress entirely. The problem was not model intelligence. The problem was that the interface had no mechanism for protecting the agent from itself.

The ACI solution was to build a search tool that returned a **capped, summarized list** of results. If your search returned more than 50 matches, the tool would suppress the output and tell the agent to narrow its query. This single design decision, which looks almost insultingly simple in retrospect, was one of the highest-leverage changes in the entire paper. It transformed a context-flooding failure mode into a natural refinement loop.

# Part Two: The SWE-Agent Paper and the Birth of the ACI

## What an Agent-Computer Interface Actually Is

The ACI is defined in the SWE-agent paper as an abstraction layer situated between a language model agent and a computer environment. The analogy to a human-computer interface (HCI) is intentional. Just as HCI research asks how to design interfaces that match human cognitive architecture, ACI research asks how to design interfaces that match LM cognitive architecture.

Human cognitive architecture involves visual pattern recognition, spatial memory, parallel attention across a screen, and the ability to skim and selectively focus. LM cognitive architecture is fundamentally different. It involves sequential token processing, sensitivity to context order and formatting, limited working memory, and a tendency to anchor on whatever information appears most prominently in the prompt. Designing a good ACI means understanding these constraints and building around them, not against them.

The SWE-agent ACI for coding tasks had four main components, and each one reflects a specific insight about how language models fail when given raw computer access.

**Search and Navigation** -- The search component replaced standard grep and find commands with purpose-built tools: find_file, search_file, and search_dir. The key difference was not syntax. The key difference was output management. Results were capped at 50. If a query exceeded that limit, the tool returned a message explaining that there were too many results and prompting the agent to refine its search.

The reason it matters is that agents, like humans under cognitive load, tend to keep doing what they are doing when they feel uncertain. The capped search tool interrupted this pattern by creating a forcing function. You cannot proceed by being vague. You must be specific.

**The File Viewer** -- The researchers tested multiple viewer configurations and found that showing 100 lines at a time was a Goldilocks number. Fewer lines (they tested 30) caused agents to lose context about the surrounding code. More lines (or the full file) caused agents to lose track of where they were. The viewer was stateful, maintained position across interactions, and prepended explicit line numbers to every visible line. Removing the cognitive task of counting lines freed up capacity for actual problem-solving.

**The File Editor With Linting** -- The editor accepted a start line, end line, and replacement text as a single operation. After every edit, the tool automatically ran a linter. If the edit introduced a syntax error, the edit was rejected before it was applied. This closed the feedback loop that causes cascading failures in naive agent implementations.

**Context Management** -- The fourth component collapsed older observations (beyond the last five turns) into single-line summaries. This kept the active context focused on recent, relevant information while preserving a compressed record of the overall trajectory.

## The Benchmark Results and What They Actually Mean

Using GPT-4 with a standard bash shell interface, the system resolved 3.97% of SWE-bench issues. Using GPT-4 with the purpose-built ACI, the system resolved 12.47% of issues. That is the 64% relative improvement, and it came entirely from interface design.

The linter integration was consistently among the highest-leverage components. The capped search was critical for preventing context flooding. The stateful file viewer with line numbers meaningfully outperformed both the raw cat command and simpler viewer designs.

The performance difference was not about model intelligence. It was about cognitive load management.

# Part Three: Anthropic's Harness Engineering (The Long-Running Agent Problem)

## Why the Context Window Boundary Is the Hard Problem

Anthropic's engineering team, working on the Claude Agent SDK and Claude Code, encountered a different problem: what happens when a task is too large to complete in a single context window?

Most real software projects are too large to fit in any context window. Even with a 200K token context window, you cannot hold the full project in mind simultaneously.

The naive solution is compaction. But compaction is not enough on its own. Anthropic's internal experiments showed that even with compaction, a frontier coding model like Opus 4.5 running in a loop across multiple context windows would consistently fail to build a production-quality web app from a high-level prompt.

The failures clustered around two patterns:

**First failure pattern: attempting to do too much at once.** When given a prompt like "build a clone of claude.ai", the agent would try to one-shot the entire application. It would begin implementing feature after feature without completing or testing any of them, run out of context window in the middle of implementation, and leave the next session with a half-implemented application, no documentation of what had been done, and no clear indication of what state the code was in.

**Second failure pattern: declaring victory too early.** After some features had been built, a subsequent agent instance would look around, see that progress had been made, and conclude that the job was done. The agent had no structured way to know what "done" actually meant for this project.

Both failures share a root cause: the agent had no persistent, structured understanding of the project's state that could survive the context window boundary and orient future sessions.

## The Two-Agent Architecture: Initializer and Coding Agent

Anthropic's solution was a two-part architecture.

The **initializer agent** is a specialized first session whose entire purpose is to set up the environment that all future coding agents will operate in. It does not write features. It creates the scaffolding. It produces three key outputs:

1. An **init.sh script** that can reliably start the development environment. Every coding agent session that follows can begin by running init.sh rather than spending tokens figuring out how to start the servers.

2. A **comprehensive feature list file**. In the claude.ai clone experiment, this meant over 200 specific, end-to-end feature descriptions. Every feature was initially marked as failing. This file serves as the project's ground truth. A coding agent starting a new session reads this file and immediately knows, with certainty, what has been built and what has not.

3. A **claude-progress.txt file** and an initial git commit. The progress file is a human-readable log that agents update at the end of every session, documenting what they worked on, what they completed, and what state they left things in.

The **coding agent** uses a different prompt: work on one feature at a time, leave the environment in a clean state, and update the progress file and git history before the session ends.

## The Feature List as a Cognitive Anchor

The feature list makes completeness explicit and unambiguous. Each feature has a passes field that is either true or false. There is no ambiguity. There is no inference required. The ground truth lives in the file.

Anthropic made a deliberate decision to store this list as JSON rather than Markdown. Empirically, models are less likely to inappropriately modify or overwrite JSON files compared to Markdown files. JSON has a rigid structure that resists casual editing.

```json
{
  "category": "functional",
  "description": "New chat button creates a fresh conversation",
  "steps": [
    "Navigate to main interface",
    "Click the 'New Chat' button",
    "Verify a new conversation is created",
    "Check that chat area shows welcome state",
    "Verify conversation appears in sidebar"
  ],
  "passes": false
}
```

The instruction was explicit: it is unacceptable to remove or edit tests because this could lead to missing or buggy functionality.

## Incremental Progress and the Clean State Requirement

Anthropic made clean state a first-class requirement. Every coding agent session ended with a git commit (with a descriptive message), an update to the progress file, and a reversion to a working state if needed.

The git commit was not just a checkpoint. It was a recovery mechanism. Version control is cognitive scaffolding, not just source management.

## Testing: The Failure Mode Nobody Likes to Talk About

Anthropic documented a failure mode that shows up in virtually every serious agentic coding project: agents marking features as complete without properly verifying them end-to-end.

The solution was to give agents access to the Puppeteer MCP server, a browser automation tool that allowed Claude to actually navigate the application, click buttons, fill forms, and verify that features worked end-to-end. The performance improvement was dramatic.

The quality of an agent's work is bounded by the quality of its feedback loops.

## The Startup Sequence: Getting Up to Speed Fast

Every coding agent session began with a standardized startup sequence: Run pwd. Read the progress file and git log. Read the feature list and choose the highest-priority incomplete feature. Run init.sh. Run the basic end-to-end test. Only after completing all of these steps would the agent begin working on a new feature.

# Part Four: OpenAI's Harness Engineering (Zero Lines of Manual Code)

## The Experiment

In late August 2025, OpenAI's Codex team started a git repository with a single constraint: no human-written code. Five months later, the repository contained approximately one million lines of code. Roughly 1,500 pull requests had been merged. A small team of three engineers averaged 3.5 pull requests per engineer per day. The central message: the bottleneck was never model capability. The bottleneck was always environment design.

## The Redefining of Engineering Work

When something failed, the fix was almost never "try harder." It was almost always "what structural piece of the environment is missing or misconfigured that is causing the agent to fail here?"

The primary job of the engineering team became enabling the agents to do useful work, not doing the work themselves.

## Repository Knowledge as the System of Record

From an agent's perspective, anything it cannot access in context while running effectively does not exist. Knowledge that lives in Google Docs, Slack threads, or people's heads is invisible to the system.

The "one big AGENTS.md" approach failed in four ways: context is a scarce resource; too much guidance becomes non-guidance; it rots instantly; it is hard to verify.

The solution was a structured docs/ directory treated as the system of record, with a short AGENTS.md file (roughly 100 lines) serving as a map that pointed to deeper sources of truth. This enabled **progressive disclosure**: agents started with a small, stable entry point and were taught where to look next.

## Application Legibility: Making the System Visible to the Agent

They made the application bootable per git worktree. They wired the Chrome DevTools Protocol into the agent runtime. They built a full local observability stack: logs, metrics, and traces exposed via LogQL, PromQL, and TraceQL. Each agent task ran on a fully isolated version of the application.

## Enforcing Architecture Without Micromanaging

The application was structured around a rigid architectural model with strictly validated dependency directions. Custom linters (written by Codex) and structural tests enforced these. The linters were custom-written to generate helpful error messages for agents with remediation instructions.

"Golden principles" were encoded directly into the repository. Recurring cleanup background tasks scanned for deviations, updated quality grades, and opened targeted refactoring pull requests.

## Throughput Changes the Merge Philosophy

Pull requests were kept short-lived. Test flakes were addressed with follow-up runs rather than blocking progress indefinitely. When agent throughput far exceeds human attention, corrections are cheap and waiting is expensive.

# Part Five: The Awesome Agent Harness Taxonomy

The Awesome Agent Harness repository catalogs seven distinct layers:

**Layer 1: Human Oversight** -- Engineers design environments and review outcomes, not writing code directly.

**Layer 2: Planning and Requirements (Spec Tools)** -- Translates human ideas into structured specifications and task DAGs. Chorus lets the AI propose task DAGs, with humans in a strict verification and approval role.

**Layer 3: Full Lifecycle Platforms** -- Manage end-to-end from requirements to delivery with human verification gates.

**Layer 4: Task Runners** -- Bridge issue trackers and coding agents. Spawn workspaces, deliver pull requests.

**Layer 5: Agent Orchestrators** -- Enable parallel execution with git worktree isolation. Tools like Vibe Kanban, Emdash, and Composio.

**Layer 6: Agent Harness Frameworks and Runtimes** -- Frameworks provide composable primitives. Runtimes provide persistent infrastructure.

**Layer 7: Coding Agents** -- The execution layer. The key insight: this layer is a commodity.

# Part Six: The Design Patterns That Repeat

## Pattern 1: Progressive Disclosure

Do not give the agent everything it might need upfront. Give it the minimum it needs to orient itself and the pointers to find more when it needs it.

## Pattern 2: Git Worktree Isolation

One agent, one worktree. Changes are made in isolation, tested in isolation, and merged only when they pass validation.

## Pattern 3: Spec First, Repository as System of Record

Agents are blind to informal knowledge. Specifications, requirements, architectural decisions, and constraints must be encoded into machine-readable files in the repository before execution begins.

## Pattern 4: Mechanical Architecture Enforcement

Human code review does not scale to agent-driven development. Encode architectural constraints as mechanical checks. Enforce invariants, not implementations.

## Pattern 5: Integrated Feedback Loops

Close the feedback loop as tightly as possible. Syntax errors at edit time. Runtime errors through observability. UI bugs through browser automation. Test failures with context.

# Part Seven: What This Actually Means for Engineers

## The Skill That Transfers

The engineers who are most effective are not the ones with the best prompting skills. They are the ones who understand how the whole system works: how context flows, where it gets corrupted, how feedback loops can be tightened, how state can be preserved across sessions, and how constraints can be enforced without micromanaging.

## The Questions You Should Be Asking

Instead of "how do I write a better prompt?" ask "what information does the agent need that it currently cannot access?" Instead of "why is the model making this mistake?" ask "what feedback loop is missing that would catch this mistake before it propagates?"

## The Commoditization of Execution

If the execution layer is a commodity, then the long-term competitive moat is not in the model. It is in the harness.

The model is what thinks. The harness is what thinks about. Getting that distinction right is the entire game.

# Part Eight: Building Your Own Harness

## The Minimal Harness

Start with a **persistent progress file**. Add a **structured task list** with verifiable completion criteria. Add **version control with descriptive commit messages** as a first-class part of every session. If building a web application, add **browser automation**.

## The Environment Audit

Ask: what information does the agent need that it does not currently have access to? Where are the points where the agent regularly gets stuck? What feedback is missing? Where is context getting polluted? What constraints need enforcement?

Each question points to a specific harness improvement. Every failure is a signal about what the environment needs.

# The Last Thing

There is a pattern in how transformative technologies get misunderstood in their early phases. The thing that captures public attention -- the raw capability -- is rarely the thing that determines who wins in the long run.

The harness is everything. The model is the reasoning engine. The harness is the context, the constraints, the feedback loops, the memory, the tools, and the scaffolding that determines what the reasoning engine can actually accomplish. Getting the harness right is not a prompt engineering problem. It is a systems engineering problem. And it is the most important engineering problem in applied AI right now.

Build accordingly.
