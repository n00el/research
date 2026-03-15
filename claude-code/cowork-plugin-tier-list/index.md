# I Tested All 21 Claude Cowork Plugins. Here's the Tier List Nobody's Publishing.

**Author:** Nav Toor (@heynavtoor)
**Date:** Feb 26, 2026
**Stats:** 1.1M views | 3.7K likes | 387 reposts | 14K bookmarks
**URL:** https://x.com/heynavtoor/status/2027054100807106829

---

## Context

Anthropic shipped 21 plugins for Claude Cowork. The original 11 launched January 30. Ten more dropped February 24. Between those two dates, $285 billion in software stock value evaporated. Thomson Reuters fell 18%. LegalZoom crashed 20%. Jefferies traders started calling it the SaaSpocalypse.

The author has been using Cowork since the week it launched, installed every plugin, tested every slash command, and run real work through each one — client proposals, financial models, content calendars, contract reviews, dataset explorations.

## What Plugins Actually Are

A plugin is a folder containing four things:
- **Skills** — domain expertise Claude draws on automatically
- **Slash commands** — structured workflows you trigger explicitly
- **Connectors** — live integrations with external tools via MCP
- **Sub-agents** — parallel workers Claude can spin up for complex tasks

## S-Tier: Install These Immediately

### 1. Data Analysis
The single most impressive plugin. Drop a CSV, type `/data:explore`. Claude reads the full dataset, summarizes every column, flags anomalies, detects data quality issues, and suggests three analyses worth running. `/data:write-query` writes and validates SQL in plain English. Connects to Snowflake, Databricks, BigQuery, Hex, Amplitude, and Jira.

Real result: Identified a pricing anomaly in a client's mid-tier plan costing ~$14,000/month in undercharged renewals from 45,000 rows across three product lines in 8 minutes. The client's data team had missed it for two quarters.

### 2. Productivity
The plugin everyone should install first. Manages tasks, calendars, daily workflows, and personal context. Type `/productivity:start` and Claude reviews your day, organizes priorities, and sets up your task list. Connects to Slack, Notion, Asana, Linear, Jira, Monday, ClickUp, and Microsoft 365.

It compounds — after a week, paired with good context files and global instructions, Claude feels like a chief of staff.

### 3. Sales
Absurdly good for prospecting, account management, or outreach. `/sales:call-prep` pulls context from CRM, researches the prospect, identifies triggers, and produces a structured briefing doc. `/sales:battlecard` builds side-by-side competitor comparisons with specific rebuttal language. Replaced 90 minutes of pre-call prep with under 10.

Connectors: Slack, HubSpot, Close, Clay, ZoomInfo, Notion, Jira, Fireflies, Microsoft 365.

## A-Tier: Genuinely Useful — Worth Installing for Your Role

### 4. Legal
The plugin that crashed $285 billion in market value. Automates contract review, NDA triage, compliance workflows, legal briefings, and templated responses. Thomson Reuters -18%, RELX -14%, Wolters Kluwer -13%, LegalZoom -20%. LexisNexis announced integration within 3 weeks. A-tier (not S) because it requires significant customization to match your org's playbook.

Connectors: Slack, Box, Egnyte, Jira, Microsoft 365. Harvey connector added Feb 24.

### 5. Product Management
`/product-management:write-spec` turns vague ideas into structured specs with user stories, acceptance criteria, and technical requirements. Uses AskUserQuestion for clarifying questions. Roadmap command synthesizes backlog, stakeholder input, and competitive context.

Connectors: Slack, Linear, Asana, Monday, ClickUp, Jira, Notion, Figma, Amplitude, Pendo, Intercom, Fireflies.

### 6. Marketing
Content drafting, campaign planning, brand voice enforcement, competitor briefings, performance reporting. `/marketing:draft-content` uses your brand-voice.md file to produce content that sounds like your brand.

Connectors: Slack, Canva, Figma, HubSpot, Amplitude, Notion, Ahrefs, SimilarWeb, Klaviyo.

### 7. Finance
PwC partnered with Anthropic on this. Journal entries, account reconciliation, financial statement generation, variance analysis, close management, audit support. Cross-app Excel→PowerPoint workflow is seamless.

Connectors: Snowflake, Databricks, BigQuery, Slack, Microsoft 365. New: FactSet, MSCI.

## B-Tier: Solid Foundation, Needs Your Customization

### 8. Customer Support
Ticket triage, response drafting, escalation packaging, KB article creation. Default templates are too generic — needs your tone, escalation criteria, SLA structure.

### 9. HR (New — Feb 24)
Offer letters, onboarding plans, performance reviews, compensation analyses. Performance review workflow structures feedback around observable behaviors. Needs substantial company-specific input.

### 10. Engineering (New — Feb 24)
Standup summaries, incident response, deploy checklists, postmortem drafting. Postmortem template is surprisingly thorough. Needs customization for severity levels and comms protocols.

### 11. Operations (New — Feb 24)
Process docs, vendor evaluations, change requests, runbook creation. Vendor evaluation workflow and decision matrix are highlights.

### 12. Design (New — Feb 24)
Critique frameworks, UX copy drafting, accessibility audits, user research plans. Accessibility audit is rigorous (WCAG compliance). Not graphic design — won't generate images.

### 16. Financial Analysis (New — Feb 24) — B-Tier
Market research, competitive analysis, financial modeling, PowerPoint creation. Cross-app Excel-to-PowerPoint is the standout.

### 17. Investment Banking (New — Feb 24) — B-Tier
Transaction doc review, comparable company analyses, pitch material. Needs firm-specific formatting and precedent transactions.

### 18. Equity Research (New — Feb 24) — B-Tier
Earnings transcript parsing, financial model updates, research note drafting. Transcript parsing is immediately useful.

### 19. Private Equity (New — Feb 24) — B-Tier
Deal sourcing, large document review, financial data extraction, scenario modeling, opportunity scoring. Document review is most immediately valuable.

### 21. Brand Voice (by Tribe AI) — B-Tier
Partner-built. Analyzes your docs/materials to distill brand voice into enforceable guidelines. Give it 10–20 examples.

## C-Tier: Promising Concept, Limited Out-of-Box Value

### 13. Enterprise Search
One query across email, chat, docs, wikis. Quality depends on connector setup and documentation structure. Connects to Slack, Notion, Guru, Jira, Asana, Microsoft 365.

### 14. Bio Research
Connects to PubMed, BioRender, bioRxiv, ClinicalTrials.gov, ChEMBL, Synapse, Wiley, Owkin, Open Targets, Benchling. Narrow user base. If you're in biotech R&D, try it.

### 15. Cowork Plugin Management
Meta-plugin for creating/customizing other plugins. Essential for admins, irrelevant for individual users.

### 20. Wealth Management (New — Feb 24) — C-Tier
Portfolio analysis, drift/tax exposure identification, rebalancing at scale. Heavily dependent on data connectors most advisors won't have configured day one.

## Strategic Pattern

- **Wave 1 (Jan 30):** Horizontal functions — productivity, sales, marketing, data, legal, finance. Triggered the SaaSpocalypse.
- **Wave 2 (Feb 24):** Vertical/industry-specific — IB, PE, equity research, wealth management, HR, engineering, design, operations. Signals platform ambitions.

Plugins are just markdown files. 2,000 GitHub stars. Private plugin marketplaces now available for enterprise. PwC building industry-specific plugins for regulated sectors.

## Quotable Lines

- "The SaaSpocalypse wasn't panic. It was price discovery."
- "A $20/month subscription doing 40% of what $150/month enterprise seats do."
- "The difference between a generic Cowork prompt and a plugin-powered prompt is the difference between asking a smart generalist and asking someone who's done your specific job for five years."
- "Anthropic shipped 10 new plugins in 25 days."
- "The plugins are just markdown files. The barrier to creating new ones is essentially zero."
