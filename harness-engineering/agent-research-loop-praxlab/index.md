# The Agent Research Loop

**Author:** Hamza Mostafa (@hamostaf04)
**Date:** 2026-03-15
**Source:** https://x.com/hamostaf04/status/2033305681395392932
**Stats:** 12 replies, 37 retweets, 333 likes, 584 bookmarks, 105K views

---

**TL;DR:** Karpathy's autoresearch is more important than people realize. Not because of the results, but because of the pattern. A tight loop with structured feedback turns a coding agent into an autonomous researcher.

## Autoresearch Is Bigger Than Pre-Training

When Karpathy released autoresearch earlier this month, coverage focused on results: 126 experiments overnight, 20 additive improvements, 11% speedup on Time-to-GPT-2. However, the more significant aspect involves the underlying pattern.

"One GPU, one file, one metric. The agent reads the training script, forms a hypothesis, modifies the code, runs the experiment for exactly 5 minutes, checks if the result improved, keeps or discards, and repeats."

This represents the scientific method compressed into a loop that an agent can execute indefinitely. And there's nothing about it that's specific to training neural networks.

## Why the Loop Works

The author previously concluded agents could execute training pipelines but lacked research capability -- training requires execution while research demands judgment. However, the actual problem was insufficient structure rather than missing judgment.

"When I gave agents open-ended freedom, they made bad decisions: changing reward functions mid-training, ignoring broken learning rate schedulers, starting from scratch every session with no memory of what worked."

Autoresearch functions as a protocol that enforces two critical elements:

**Discipline:** One change at a time. Hypothesis before experiment. Confirm or refute after. This sounds obvious, but agents without this structure will change three things at once.

**Memory:** The git history is a lab notebook. The agent can see what it already tried, what worked, what didn't.

The optimal approach balances autonomy with constraints: human sets direction and constraints, agent does exhaustive exploration within those bounds.

## What Changes for Humans

When experimental costs approach zero, the bottleneck shifts entirely to the decisions that happen before the loop starts. Which task? Which model? Which metric?

Human decisions become increasingly strategic. Agents excel at the 50th hyperparameter sweep, the careful ablation, the 12th self-distillation round -- precisely what agents are designed for.

## Where This Goes

Two agent loop flavors are emerging:

1. **Closed-loop optimization** with defined endpoints (like AlphaEvolve)
2. **Open-ended research** without finish lines (like Autoresearch)

The interface for programming agent sessions requires specification of the metric, the levers, the feedback loop, and the constraints -- potentially becoming a research specification rather than conversational prompts.

## What We Built: PraxLab

PraxLab is an open-source harness featuring self-contained workspaces with program.md files containing agent instructions, mutable training scripts, and experiment tracking.

"The lab CLI is the structured memory layer. 5 commands, SQLite, zero dependencies. Before each experiment the agent runs 'lab context' and 'lab failures'."

Over 48+ hours, four Claude Code instances ran 550+ experiments with zero intervention, producing highlights including:

- **RL**: 93% on MATH level 4-5 with discovered MAX_TOKENS scaling laws
- **SFT**: Self-distillation matched RL performance at half cost
- **Tool routing**: 0.76 -> 0.94 on 6-tool routing
- **Prompt co-evolution**: 0.75 -> 0.94 over four generations

## Try It, Extend It

Clone it, edit Section 1 of a program.md, spin up your favourite coding agent. The repository welcomes community contributions of new workspaces for different frameworks and tasks.

**Repository:** github.com/Hamza-Mos/praxlab
