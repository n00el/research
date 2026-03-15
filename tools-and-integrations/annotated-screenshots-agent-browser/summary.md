# New: Annotated Screenshots for agent-browser

**Author:** Chris Tate (@ctatedev)
**Date:** Feb 19, 2026
**Source:** https://x.com/ctatedev/status/2024346489456144735

## Key Takeaways

- Text snapshots of web pages are fast but miss visual-only elements like canvas content, icon-only buttons, and elements lacking ARIA labels.
- Pure screenshots give visual context but lose the structural and semantic information agents need to act on elements.
- Annotated screenshots bridge the gap by overlaying numbered labels on each interactive element, linking the visual rendering to the accessibility tree.
- The feature is available via `agent-browser screenshot --annotate`, making it a single-command addition to existing agent workflows.
- This approach directly maps what an agent "sees" visually to what it can programmatically interact with, solving a core grounding problem in browser agents.

## One-Line Summary

Annotated screenshots that overlay numbered labels on interactive elements solve the fundamental disconnect between visual page context and actionable structure in browser agents.

## Topics

browser agents, accessibility, web automation, computer use, agent tooling
