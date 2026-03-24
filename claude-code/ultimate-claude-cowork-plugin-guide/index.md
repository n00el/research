# Ultimate Claude Cowork Plugin Guide: 11 Plugins You Can Steal + 5 Prompts

**Author:** Corey Ganim (@coreyganim)
**Date:** March 20, 2026
**Source:** https://x.com/coreyganim/status/2035004622398538122
**Stats:** 3 replies, 50 reposts, 407 likes, 1.1K bookmarks, 169.4K views

---

Plugins are an INSANELY underrated Claude Cowork feature.

Almost no one is using them correctly.

Plugins are what turn Claude from a chatbot into a specialist for your specific role.

They include commands that trigger entire workflows (research, formatting, delivery), and hands you back the finished work.

This article breaks down exactly how plugins work, how they differ from Skills, and gives you 5 copy-paste prompts to build your own.

PS: We built an onboarding plugin that sets up Claude Cowork for YOUR business in under 10 minutes.

It's included free inside our Build With AI community.

Join 8,004 others on the waitlist: https://return-my-time.kit.com/1bd2720397

---

# What Plugins Actually Are

A plugin is a bundle. It packages four things together:

- **Skills** — instructions that teach Claude how to do specific tasks
- **Connectors** — links to external tools (Google Drive, Slack, Notion, etc.)
- **Slash commands** — quick actions you trigger with /command
- **Sub-agents** — parallel workers that handle pieces of a task

Instead of setting up each piece individually, you install one plugin and get a ready-to-go setup from the first conversation.

Think of it this way: skills are the individual capabilities. Plugins are the job description that bundles the right skills together.

## How Plugins Differ from Skills

This is where most people get confused.

**Skills** are single-purpose instructions. One skill might teach Claude how to write in your voice. Another might teach it how to format a proposal. You can use skills without a plugin.

**Plugins** bundle multiple skills together with the connectors and commands needed to run a complete workflow.

Example:

- **Skill:** "Write status update in my tone"
- **Plugin:** Pulls active projects from Notion, writes status updates for each client using your communication skill, and drops the drafts into Gmail. One command, five clients handled.

Skills are Lego blocks. Plugins are the assembled set.

# The 11 Official Anthropic Plugins

Anthropic shipped these on January 30, 2026. All free, all open source:

- **Sales** — call prep, competitive research, account plans
- **Marketing** — SEO audits, email sequences, content briefs
- **Legal** — contract review, compliance checklists, risk analysis
- **Finance** — financial modeling, budget analysis, forecasting
- **Customer Support** — ticket response, escalation workflows
- **Product Management** — PRDs, roadmap planning, user research
- **Data Analysis** — dashboards, data cleaning, visualization
- **Enterprise Search** — cross-platform search across company tools
- **Biology Research** — literature review, experiment design
- **Productivity** — task management, meeting prep, scheduling
- **Plugin Create** — build your own plugins from scratch

Browse them all at github.com/anthropics/knowledge-work-plugins

# How to Install a Plugin

Open Cowork → Plugins (sidebar) → Click "+" → Browse available plugins → Click "Add plugin"

That's it. Plugins you add are saved locally to your machine.

To use a skill from your plugin, type "/" in the chat to see available commands, or click the "+" button and select from your installed plugins.

# The Most Useful Slash Commands

Here are commands worth bookmarking from the official plugins:

**Marketing Plugin:**

- /marketing:seo-audit — Analyzes any URL for SEO issues
- /marketing:email-sequence — Generates a multi-email sequence
- /marketing:competitive-brief — Researches competitors and outputs a brief

**Sales Plugin:**

- /sales:call-prep — Researches a prospect and creates a call brief
- /sales:account-plan — Builds a strategic account plan
- /sales:objection-handling — Generates responses to common objections

**Legal Plugin:**

- /legal:contract-review — Analyzes a contract for risks
- /legal:compliance-check — Checks against regulatory requirements

**Data Plugin:**

- /data:clean-dataset — Standardizes messy data
- /data:visualize — Creates charts from data

# How Connectors Work

Connectors link Claude to external tools. Without them, Claude can only work with what you paste in. With them, Claude can pull live data.

Available connectors include:

- Google Drive, Gmail, Google Calendar
- Slack, Notion, Asana
- DocuSign, Salesforce, FactSet
- Linear, Monday.com, and more

To connect: Settings → Connectors → Browse connectors → Search for your tool → Connect

Once connected, Claude can search your Drive, read your emails, check your calendar, and act on that data within a workflow.

# How to Customize a Plugin

The default plugins are starting points. Click "Customize" on any installed plugin and Claude walks you through adjusting it:

- Swap connectors to match your tool stack
- Add your company's terminology
- Adjust workflows to how your team operates
- Add custom skills specific to your process

Prompt to use:

```
Customize this plugin to match how my team actually works. We use [tools]. Our process for [workflow] is [description]. Update the skills and commands accordingly.
```

# How to Build Your Own Plugin

Use the Plugin Create plugin (meta, I know). Or build from scratch with this structure:

```
my-plugin/
├── plugin.json        # Manifest file
├── commands/          # Slash commands
│   └── my-command.md
├── skills/            # Individual skills
│   └── my-skill/
│       └── SKILL.md
└── .mcp.json          # Connector config
```

Prompt to start:

```
I want to build a plugin for [your workflow]. Walk me through creating the plugin structure, skills, and commands. My workflow involves [describe the steps].
```

# Copy-Paste Prompts for Common Use Cases

Create a client management plugin:

```
Build me a client management plugin that pulls active projects from Notion, writes weekly status updates using my communication style, and drafts them in Gmail. Include a /send-updates command.
```

Create a content repurposing plugin:

```
Build a content repurposing plugin with skills for: turning podcast transcripts into LinkedIn posts, creating Twitter threads from articles, and extracting clips suggestions from video scripts.
```

Create a research plugin:

```
Build a competitive research plugin that searches the web, organizes findings into a structured brief, and saves it to my Google Drive. Include /research-company and /market-analysis commands.
```

# What Plugins Actually Unlock

Here's the main benefit:

Before plugins, you had to manage each piece manually. Connect your tools. Load your skills. Remember which prompt to use. Repeat for every task.

With plugins, you set it up once and run it forever. One command triggers an entire agentic workflow and hands you back the finished work.

The people who figure this out first will operate like they have a team of 10.

The people who don't will wonder why everyone else is moving so fast.

PS: We built an onboarding plugin that sets up Claude Cowork for YOUR business in under 10 minutes.

It's included free inside our Build With AI community.

Join 8,004 others on the waitlist:
https://return-my-time.kit.com/1bd2720397
