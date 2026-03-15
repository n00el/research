# How to Set Up Claude Cowork the Right Way (So It Actually Does Your Work While You Step Away)

**Author:** Nav Toor (@heynavtoor)
**Date:** Feb 25, 2026
**Stats:** 8.2M views | 16K likes | 2K reposts | 73K bookmarks
**URL:** https://x.com/heynavtoor/status/2026717574776631556

---

## What Is Cowork?

Cowork is not a chatbot. It lives on your desktop, reads/writes to folders on your computer, creates Word documents, builds spreadsheets with working formulas, installs specialist plugins for your exact job. When it doesn't have enough info, it asks — instead of guessing.

> Cowork is what happened when Anthropic took the power of Claude Code — their agentic developer tool — and rebuilt it for the rest of us. No terminal. No command line. No code.

The author describes covering ~60% of their knowledge work with a loop: describe what's needed → Cowork asks 3 questions → answer them → come back 20 minutes later to a finished document.

## The 5 Core Features (Ranked by Impact)

### 1. File System Access

Claude reads and writes files in a folder on your computer. No more upload/download loops. Select a folder and Claude reads everything inside it, saves outputs directly there.

**Setup:**
1. Download desktop app from claude.com/download (macOS or Windows)
2. Need paid plan: Pro ($20/mo) or Max ($100/mo+)
3. Open app → switch to Cowork tab
4. Click folder icon → select a local folder

**Pro Tip — Context Files Strategy:**
Create a "Claude Context" folder with three files:
- `about-me.md` — who you are, what you do, your role, what success looks like
- `brand-voice.md` — how you communicate, your phrases, what sounds wrong, tone
- `working-style.md` — how you want Claude to behave, questions first? short/long outputs? formats?

> Stop thinking about better prompts and start thinking about better files.

**First prompt:** "Read all the files in this folder completely. Then give me a summary of what you know about me, how I work, and what context you have access to."

### 2. AskUserQuestion

Cowork asks YOU questions instead of guessing. When it needs more info, it stops and generates a form with multiple-choice questions and specific options.

**How to use:** Add to end of any prompt:
> DO NOT start working yet. First, ask me clarifying questions so we can define the approach together. Only begin once we've aligned.

**First prompt template:**
> I want to [YOUR TASK] so that [WHAT GOOD LOOKS LIKE]. First, read all uploaded files completely before responding. DO NOT start executing yet. Ask me clarifying questions (use AskUserQuestion) to refine the approach. Only begin work once we've aligned.

> ChatGPT trained you to write better prompts. Cowork trains you to give better context. One is a skill that deprecates. The other compounds.

### 3. Plugins

Pre-built specialist packs that make Claude an expert instantly. Bundles of skills, slash commands, and sub-agents for specific job functions. 11 official plugins shipped January 2026:
- Productivity, Marketing, Sales, Finance, Data Analysis, Legal, Product Management, Customer Support, Enterprise Search, Biology Research, and more being added.

**Market impact:** When the Legal plugin launched, Thomson Reuters dropped 16% in a single session. LegalZoom fell 20%.

**Install:** Click "+" in chat bar → Plugins → browse library, or go to claude.com/plugins.

**Example prompts after installing:**
- Productivity: `/productivity:start Let's review what I need to get done today and set up my task list.`
- Marketing: `/marketing:draft-content Write a LinkedIn post about [topic]. Match the voice from my brand-voice.md file.`
- Data: `/data:explore I've uploaded a CSV in this folder. Give me a summary, flag anomalies, suggest three analyses.`

### 4. Instructions (Global & Folder)

Permanent memory that loads automatically at start of every session. Global instructions apply everywhere. Folder instructions apply per-project.

**Setup:** Settings > Cowork > Edit next to Global Instructions.

**What to include:**
- Your name, role, what you do
- Communication preferences ("concise, direct — no padding")
- Output defaults ("Always save as .docx unless I say otherwise")
- Working style ("Always ask clarifying questions before starting")
- Things to avoid ("Never use bullet points unless requested")

Folder instructions are perfect for client work — each client folder gets its own brief that loads automatically.

### 5. Connectors

Live integrations with Slack, Drive, Notion, and 50+ tools. Free on all plans. Link tools once, Claude references live data mid-conversation.

**Setup:** Settings > Connectors → browse 50+ integrations → Add → Authenticate. One-time setup.

**Example prompts:**
- Slack: "Search my Slack messages from the last 7 days and summarize anything I need to follow up on. Organize by urgency."
- Google Drive: "Find the most recent document about [project name] in my Drive. Read it and tell me the three most important things."

## Where Cowork Falls Short

- **No cross-session memory.** Every session starts fresh. Workaround: keep context in markdown files + solid global instructions.
- **Tasks stop if you close the app.** Put computer to sleep instead — session survives that.
- **Usage goes faster than regular chat.** Complex tasks consume more allocation. Monitor Settings > Usage. Max plan may be worth it for heavy use.
- **Desktop only.** No mobile, no browser, no sync across devices. Use cloud-synced folder for file consistency.
- **No image generation.** Use Gemini Imagen for images.
- **Research preview.** Agent safety still under development. Enable deletion protection, review plans before executing on sensitive files.

## Your First 30 Minutes

| Time | Action |
|------|--------|
| 0–5 min | Install desktop app, sign in, get Pro ($20/mo), find Cowork tab |
| 5–10 min | Create `about-me.md`, `brand-voice.md`, `working-style.md` in a "Claude Context" folder |
| 10–15 min | Set global instructions in Settings > Cowork |
| 15–20 min | Run first Cowork task with context folder selected |
| 20–25 min | Install a plugin (Productivity, Marketing, or Sales) |
| 25–30 min | Connect one tool (Slack, Google Drive, or Notion) |

## Key Takeaway

> ChatGPT trained you to write better prompts. Cowork trains you to build better context. One is a skill that depreciates. The other compounds.
