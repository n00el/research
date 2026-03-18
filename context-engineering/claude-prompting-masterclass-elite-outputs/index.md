# Claude Prompting Masterclass: How to Get Elite Outputs

**Author:** AI Edge (@aiedge_)
**Date:** 2026-03-16
**Source:** https://x.com/aiedge_/status/2033546384172056751
**Stats:** 8 replies, 44 retweets, 327 likes, 931 bookmarks, 169K views

---

Prompt engineering is the #1 AI skill to learn right now.

## Common Pitfalls

### 1. No Context

This is the biggest mistake. When you prompt Claude without context, you're essentially asking a world-class expert to help you with zero background information.

The output will always be generic because the input was generic.

Think of it like hiring the best consultant in the world and just saying, "give me advice." Advice about what? For who? In what situation?

Context is everything. The more relevant information you feed Claude upfront (your goal, your audience, your constraints, your situation), the more surgical and useful the output becomes.

### 2. Being Too Vague

Vague prompts produce vague answers. It's that simple.

Most people write prompts the way they'd type a Google search: short, broad, and keyword-driven. Claude isn't a search engine; it's a reasoning model that predicts the next logical output based on the inputs you provide.

The more specific your input, the more precise your output.

If you want a specific result, you need to ask a specific question. Define the format you want, the length, tone, etc.

TLDR: Leave nothing open to interpretation.

### 3. Not Iterating

The biggest misconception about prompting is that a great output should come from a single prompt.

It rarely does.

The best prompters treat Claude like an ongoing conversation. They get an initial output, identify what's missing, and refine. Each iteration gets closer to exactly what they need.

## The Foundations

According to the Anthropic team, ten steps make a Claude prompt great. Boiled down to five essential components:

### 1. Task Context

Set the role and task upfront. This is the foundation of every good prompt. Claude needs to know who it is and what it is doing before anything else.

Example: you are [insert role] and your job is to [insert task]

### 2. Background Data

Give Claude something to work with. Upload relevant docs, files, or context profiles. The more relevant information you provide, the more precise the output.

### 3. Detailed Task Description & Rules

Expand on your goal with specific constraints and guidelines. Don't just say what you want -- say exactly how you want it done.

Explain your end goal to Claude and have the model propose a plan to get there.

### 4. Examples

The single biggest unlock for output quality. If you have a desired output format or a previous response you loved, show it to Claude using the `<example>` tag. Nothing calibrates Claude models faster than a concrete reference point.

### 5. Output Formatting

Tell Claude exactly how you want the response structured before it replies.

Examples: bullet points, infographics, table format, and phrases like "condense your response to less than 500 words."

## Advanced Prompting Techniques

### 1. Structured Prompting (JSON & XML Tags)

Structured prompting is the single biggest upgrade you can make to your LLM inputs.

Instead of writing prompts in plain English text, you organize them using XML tags or JSON formatting. Think of this as speaking the same "language" as Claude.

XML tags example:

```xml
<role>You are an expert marketing strategist</role>
<context>I run a B2B SaaS company targeting HR managers</context>
<task>Write a 90-day go-to-market strategy</task>
<format>Numbered list with a brief explanation for each step</format>
```

JSON formatting example:

```json
{
  "role": "expert marketing strategist",
  "context": "B2B SaaS company targeting HR managers",
  "task": "Write a 90-day go-to-market strategy",
  "format": "Numbered list with brief explanations"
}
```

### 2. Reverse Prompting

Most people tell Claude what to do. Reverse prompting flips that entirely.

Instead of writing the prompt yourself, you ask Claude to question you to gather the context it needs.

Example: "Ask me 10 questions to gather all the context necessary to build me a personalized business plan."

This is one of the most underrated techniques in prompting. Use it whenever you're stuck, whenever the outputs feel off, or whenever you're approaching a new type of task for the first time.

### 3. Deep Thinking Triggers

Claude has extended reasoning capabilities, but most people never activate them.

By default, Claude gives you a fast response. For complex problems, strategy, analysis, or anything requiring genuine reasoning, you want Claude to think more deeply before responding.

Add any of these to your prompts:

```
Think deeply before responding.
Take your time and reason through this step by step.
Consider multiple angles before giving me your answer.
```

### 4. Chain Prompting

Chain prompting is the art of breaking one big task into a sequence of smaller, connected prompts.

Instead of asking Claude to do everything in one go, you build toward the final output step by step.

Example:

```
Prompt 1: "Analyse the biggest challenges facing [business/industry]"
Prompt 2: "Based on those challenges, identify the top 3 opportunities"
Prompt 3: "Now build a 90-day action plan to capitalise on opportunity #1"
Prompt 4: "Turn this into an executive summary I can present to stakeholders"
```

This works because each step gives Claude full context from the previous one.

### 5. Feedback Looping

Most people accept the first output they get. This is wrong.

Feedback looping is the practice of actively critiquing Claude's output and asking it to improve, until the result is exactly what you need.

```
"This is good, but the tone is too formal. Rewrite it to sound more conversational."
"The third point is weak. Expand on it with a concrete example."
```

## Anthropic's Tips & Tricks

### Claude Prompting Best Practices Doc

- Be clear and direct
- Context is key
- Use examples (3-5 is ideal)
- Claude was trained on XML tags -- use them
- Put longform data at the top: place long documents and inputs near the top of your prompt, above your query, instructions, and examples

### How to Prompt Engineer

- Ask Claude to quote sources before answering
- Keep context tight -- more isn't always better
- Have Claude self-check before finishing
- Bad latency? Switch models, don't switch your prompt

### Other Anthropic Prompting Tips

- Build system prompts
- Tools should be minimal
- Use a Scratchpad: telling Claude to use a scratchpad to show its work improves accuracy. End users do not need to see the scratchpad, but it helps Claude reason more carefully before producing its final answer.
- Define success criteria: tell Claude exactly what success looks like (like you would with an employee)
