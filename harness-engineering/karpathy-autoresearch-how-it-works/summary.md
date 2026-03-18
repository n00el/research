# Karpathy's Autoresearch Went Viral. Here's How It Works

**Author:** Alexey Grigorev (@Al_Grigor)
**Date:** 2026-03-16
**Source:** https://x.com/Al_Grigor/status/2033558987258429582

## Key Takeaways

- Autoresearch differs from AutoML by having the agent edit the training script itself rather than searching a predefined parameter space
- Three-file architecture: prepare.py (fixed evaluation), train.py (agent-editable model code), program.md (natural-language instructions governing agent behavior)
- Strict time budgets per experiment and automatic revert on failure enforce disciplined exploration
- 110 successful changes in 12 hours, improving val_bpb from 0.862415 to 0.858039
- The pattern generalizes beyond training: AutoVoiceEvals applied it to prompt optimization, improving a scheduling agent from 25% to 100% success rate
- The author proposes applying autoresearch to writing style optimization, treating the prompt itself as the artifact being optimized

## One-Line Summary

Karpathy's autoresearch is a three-layer harness (fixed evaluation + editable code + natural-language instructions) that turns LLM agents into autonomous researchers running continuous experiment loops.

## Topics

autoresearch, Karpathy, experiment loops, harness engineering, automated optimization
