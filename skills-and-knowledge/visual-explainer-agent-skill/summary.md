# visual-explainer: Agent Skill for Styled HTML Output

**Author:** nicobailon
**Date:** 2026-02-16
**Source:** https://github.com/nicobailon/visual-explainer

## Key Takeaways

- Replaces ASCII art and box-drawing terminal output with self-contained HTML pages featuring real typography, dark/light themes, and interactive Mermaid diagrams with zoom/pan.
- Provides 8 slash commands covering diagrams, diff reviews, plan audits, project recaps, slide decks, fact-checking, and Vercel deployment -- all generating browser-viewable HTML.
- Automatically intercepts complex terminal tables (4+ rows or 3+ columns) and renders them as HTML instead, requiring no explicit user action.
- Supports a `--slides` flag on any command to produce magazine-quality slide decks instead of scrollable pages.
- Works across Claude Code (marketplace plugin), Pi (manual skill install), and OpenAI Codex -- showing a cross-agent skill distribution pattern worth studying.
- Uses smart routing: Mermaid for flowcharts, CSS Grid for architecture, HTML tables for data, Chart.js for dashboards -- no user configuration needed.
- Extremely popular (5.9k stars in ~3 weeks) indicating strong demand for better agent output presentation.

## One-Line Summary

A Claude Code / agent skill that intercepts complex terminal output and renders it as polished, interactive HTML pages with diagrams, tables, and slide decks.

## Topics

agent skills, developer tools, claude code plugins, HTML visualization, mermaid diagrams, terminal UX
