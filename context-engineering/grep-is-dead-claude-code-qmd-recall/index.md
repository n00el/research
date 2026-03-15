# Grep Is Dead: How I Made Claude Code Actually Remember Things

**Author:** Artem Zhutov (@ArtemXTech)
**Date:** March 2, 2026
**Source:** https://x.com/ArtemXTech/status/2028330693659332615
**Stats:** 2,707 likes, 214 retweets, 95 replies, 9,155 bookmarks

---

The post introduces a memory system for Claude Code that solves the problem of losing context between sessions, using **QMD** — a local search engine created by Shopify CEO Tobias Lutke — as the backbone for persistent memory.

## The Problem

Claude Code loses all context between sessions. Each new conversation starts from scratch with no recall of prior work, decisions, or reasoning.

## QMD Search Engine

The solution uses QMD to index Obsidian vaults. It supports three search modes:
- **BM25** -- deterministic full-text search (exact keyword matching)
- **Semantic** -- meaning-based search (understands intent even when exact words differ)
- **Hybrid** -- combines both approaches for best results

## /recall Skill

A custom Claude Code tool that enables temporal, topic-based, and graph-based context retrieval. This lets Claude Code pull in relevant past context on demand.

## Session Management

The system automatically exports and indexes over **700 Claude Code conversations**, making them all searchable and retrievable.

## Core Insight

> "Your notes stop being passive...they actually start doing things."

Notes and documentation stop being passive artifacts -- they become active, searchable context that works across tool changes and platforms.

## Resources

The post includes a link to the QMD GitHub repository and a 42-minute video walkthrough explaining the full setup.

The high bookmark count (9,155) relative to likes (2,707) suggests highly actionable content that resonated as a practical, implementable solution.
