# Karpathy's Autoresearch Went Viral. Here's How It Works (and One Idea to Try)

**Author:** Alexey Grigorev (@Al_Grigor)
**Date:** 2026-03-16
**Source:** https://x.com/Al_Grigor/status/2033558987258429582
**Stats:** 2 replies, 21 retweets, 171 likes, 279 bookmarks, 13K views

---

## Core Idea

At a high level, autoresearch automates something that normally takes a large amount of human time: running experiments and iterating on models.

Autoresearch delegates the experimental loop to an agent that runs continuously, performing many small experiments autonomously while gradually improving the model.

The system resembles AutoML but differs fundamentally: instead of selecting from predefined parameter spaces, the model edits the training script itself and proposes new ideas for the architecture or training procedure.

## Repository Structure

The implementation relies on three key files:

- **prepare.py**: Contains fixed experimental components -- data preparation, dataset downloads, and evaluation logic that agents cannot modify.
- **train.py**: Houses model implementation and training loops; this file the agent edits when proposing experiments.
- **program.md**: Contains natural language instructions for agent behavior, described as "research org code written in English."

The system establishes a baseline by creating a Git branch, running the unmodified script, and recording initial metrics. It then enters an experiment loop: editing train.py, running experiments, extracting metrics, and keeping changes only if results improve.

## Three Programming Layers

The project operates across distinct abstraction levels:

- Traditional code defining environmental rules
- Python code representing modifiable models
- Natural language instructions governing agent behavior

Instead of directly improving the model, the human is programming the experimental process using natural language.

## Optimization Process

Each experiment has a strict time budget, so runs cannot expand indefinitely. Only modifications improving the metric are retained; failing changes automatically revert.

The system optimizes validation bits per byte (val_bpb), a metric remaining comparable across tokenizer and vocabulary changes.

## Results

Karpathy reported that the system produced 110 successful changes in about twelve hours, improving the validation metric from 0.862415 to 0.858039.

## Others Experimenting

**Autosearcher** runs multiple agents in parallel, rediscovering techniques like Kaiming initialization through pure experimentation.

**AutoVoiceEvals** applies the pattern to prompt optimization, improving a scheduling agent's success rate from 25 percent to 100 percent.

## Project Idea: Writing Style Optimization

The author proposes automating style guide refinement using an autoresearch loop, treating the prompt itself as the optimized artifact.

Each iteration would: modify the style guide, generate sample outputs, compare to reference texts, evaluate similarity, and keep improvements.

This approach turns prompt tuning into an automated search process.

## Why It Matters

LLMs can now participate directly in the research workflow. They can read code, propose modifications, run experiments, analyze results, and generate the next hypothesis.

The significance lies in reorganizing experimentation: humans define constraints while systems explore autonomously.
