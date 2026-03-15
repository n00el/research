# Pal: a personal context-agent that learns how you work

**Author:** Ashpreet Bedi (@ashpreetbedi)
**Date:** 5:11 PM, Mar 6, 2026
**Source:** https://x.com/ashpreetbedi/status/2029953139856531528
**Stats:** 13 replies, 51 reposts, 608 likes, 1,417 bookmarks, 58,450 views

---

![Pal header image](image-1.png)

You have a meeting with Sarah in an hour. You need to prep. You check your calendar for the agenda. Search your email for the last thread. Open your notes for what you captured last time. Find the project brief in your files. Copy the relevant bits into a prompt and hand it off to claude.

Congratulations, you just did an agent's job. You navigated across four systems, found the right context, assembled it into one giant prompt, and handed it to Claude. Something an agent should be able to do by itself.

Your AI has memory. But you're still the integration layer between your tools.

# A Different Workflow

You tell Pal: "Prepare for my meeting with Sarah."

Pal classifies this task as meeting prep. It searches its knowledge, a metadata index of where things are, and finds information about Sarah in the contacts table, there's a project record linked to her, and a `partnership-brief.md` in the projects directory. It checks its learnings and finds a strategy for meeting prep tasks: check calendar for the agenda, email for recent threads, notes for prior context. Then it navigates:

- Calendar → finds the meeting, pulls the agenda, notes it's a partnership review
- Gmail → searches for the last thread with Sarah, who sent updated terms yesterday
- SQL → queries pal_notes and pal_projects for everything tagged "sarah"
- Files → reads partnership-brief.md for the project context

Four sources. Each processed independently, then synthesized into a coherent brief: here's the agenda, here's what Sarah sent yesterday, here's the project status, here are the open questions from your notes. You didn't search anything. You didn't copy-paste. You didn't tell Pal where to look. It knew, because it's been building a map of where things are and learning how you work.

> But what if Pal had already surfaced the brief before you needed it? More on that later.

# Context Agents

What Pal is doing has a name. It's navigating a context graph: a set of heterogeneous systems where your data actually lives. SQL tables, local files, email threads, calendar events, web search.

The best analogy for this navigation is Claude Code. When you point Claude Code at a codebase, it doesn't embed every file into a vector store and run similarity search. It navigates. It reads the directory structure, follows imports, finds the relevant files, builds a map of where things live. It gets faster and more accurate the more it explores.

Pal does the same thing for your life. Your notes, your emails, your projects, your documents. A context agent learns where things are and how to find them across all of these.

Compare this to the traditional knowledge-agent approach: embed everything into a vector database and run similarity search. You ask about Sarah, the agent returns 15 semantically similar chunks. Some about Sarah, some about a random meeting she was in, some from a project she's mentioned in once. The response is long, unfocused, and misses the email from yesterday because emails weren't embedded yet.

The difference is navigation and learning. Knowledge agents search for chunks in an unorganized, growing pile. Context agents navigate across systems and get smarter over time.

# The Context Graph

A context graph is a map of where information lives and how to navigate it. Most agents treat your data as something to be searched. They embed it, chunk it, and retrieve it by similarity. The context graph treats your data as something to be navigated. Structured memory in SQL, documents in markdown files, threads in email, events in calendar. Each system is queried on its own terms, not flattened into a pile of vectors. The result is retrieval that's precise, current, and gets better the more you use it.

## Four systems make up Pal's context graph.

1. SQL database for structured memory. Notes, people, projects, decisions, stored in pal_* tables that Pal creates on demand. Tags are the cross-table connector: a note about a meeting with Sarah about Project X gets tagged ['sarah', 'project-x'], making cross-table queries natural. Pal owns the schema and evolves it as your data grows. Language models are remarkably good at SQL, and letting Pal manage its own structured memory is a real unlock. With this, Pal can answer "What do I know about Sarah?" by querying across notes, people, and projects. Facts persist in tables, linked by tags, queryable at any time.

> Key point to note: Pal creates and manages its own database.

2. Files are how you configure Pal's behavior. Brand voice guides, project briefs, meeting notes, reference material. Plain markdown files in a context/ directory, read on demand. Drop a voice/email.md in context and Pal reads it before drafting emails. Edit it, and Pal's behavior changes

> Key point to note: File's configure Pal's behavior. Edit a file, change how it works.

Pal reads your documents the same way a coding agent reads a codebase. The source material is always current. And the context directory grows over time: scheduled tasks and interactive sessions both write output back into meetings/ and projects/.

```markdown
context/
├── about-me.md           # Background, goals, active projects
├── preferences.md        # Working-style config, file conventions
├── voice/                # Writing tone guides per channel
│   ├── email.md
│   ├── x-post.md
│   └── linkedin-post.md
├── templates/            # Scaffolds Pal fills per use
│   ├── meeting-notes.md
│   └── weekly-review.md
├── meetings/             # Saved meeting notes and weekly reviews
└── projects/             # Project briefs and docs
```

3. Knowledge is the map. As Pal works, it builds an index of where things live: what files exist, what tables have been created, what sources are available, where specific information was found before. When Pal discovers that Project X information lives across pal_projects, pal_notes, partnership-brief.md, and a Gmail thread, it saves a Discovery: entry. Next time someone asks about Project X, it goes directly to those four places. No searching. No guessing.

> Key point: The map gets more accurate every session. By the tenth interaction, Pal knows where to look before you ask.

4. Learnings are the compass. Pal saves what works and what doesn't. Retrieval strategies that produced good results. Behavioral patterns it's noticed. Corrections you've made. When Pal figures out that meeting prep works best by checking calendar first, then email, then notes, then files, it saves that pattern and uses it every time. When you correct it ("don't create events with external attendees without asking"), it saves a Correction: that always takes priority over everything else.

> Key point: Learnings help Pal avoid making the same mistake twice. The compass is built from your feedback.

## The core insight

Context Agents improve by navigating context better, the prompt stays the same. The tenth interaction benefits from structured memory, a map of where things are, a compass of what strategies work, and remembered preferences. All of that fits in the same prompt.

## Pal's Execution Loop

Every interaction with Pal follows the same five steps:

- **Classify:** What kind of request is this? Classification determines which sources to check.
- **Recall:** Search knowledge, learnings and determine the context to retrieve, scoped to the classified intent from step 1.
- **Retrieve:** Pull context from the right sources, identified by step 2.
- **Act:** Execute using the retrieved context.
- **Learn:** Save what worked for next time.

Classification comes first. This is what makes recall targeted. If Pal knows the user wants meeting prep before it searches, it looks for the person, the calendar event, and the project file. The query is scoped before it starts.

## Scheduled Tasks

A context agent that only runs when you ask is still a tool. Compounding happens in the background. Scheduled tasks turn Pal from a capable assistant into something that's already done the work before you show up.

Pal ships with five scheduled tasks:

- Daily briefing (8 AM weekdays)
- Inbox digest (12 PM weekdays)
- Weekly review (5 PM Friday)
- Context refresh (8 AM daily)
- Learning summary (10 AM Monday)

Each task is a python script that registers a cron schedule managed by Pal's AgentOS. The daily briefing one looks like this:

```python
mgr = ScheduleManager(get_postgres_db())
mgr.create(
    name="daily-briefing",
    cron="0 8 * * 1-5",
    endpoint="/agents/pal/runs",
    payload={
        "message": "Good morning. Quick briefing: "
        "1. Today's calendar with times. "
        "2. Unread or flagged emails. "
        "3. Open priorities from recent conversations."
    },
    timezone="America/New_York",
    if_exists="update",
)
```

The payload is the task definition. A scheduled message to the same agent/run endpoint that handles interactive conversations.

## Governance Boundary: External Impact

Currently, Pal's governance follows the rule: never take actions that affect other people without confirmation.

- **Email:** Send tools are excluded. Pal can only create drafts.
- **Calendar:** Personal events are created freely. Events with external attendees require confirmation because they send invites.
- **Files and SQL:** Written freely. File deletion is disabled at the code level.

Most of what you do with a personal agent should have no external impact. The few things that do are gated at this boundary. Scheduled tasks follow the same governance: the daily briefing reads your calendar, it never modifies it. The weekly review saves a file, it never sends anything.

> I'm also looking to add @approval flows to Pal, which can give it a bit more room to make decisions but with admin oversight. More on that soon.

## Get Started

Pal is open source, checkout the [repo](https://github.com/agno-agi/pal) for more details.

```bash
git clone https://github.com/agno-agi/pal.git
cd pal
cp example.env .env
# Edit .env and add your OPENAI_API_KEY
docker compose up -d --build
# Load context file metadata into the knowledge map
docker compose exec pal-api python context/load_context.py
```

Connect to the web UI at [os.agno.com](https://os.agno.com), add your local endpoint `http://localhost:8000`, and try:

```plaintext
Met with Sarah Chen from Acme Corp. She's interested in a partnership.
What do I know about Sarah?
Check my latest emails
What's on my calendar this week?
Draft an X post in my voice about AI productivity
Save a summary of today's meeting to meeting-notes.md
Research web trends on AI productivity
```

# Adding Capabilities

Pal starts with SQL + Files + Exa web research (free via MCP). That's enough to get started but the real surface area is much larger. Agno ships with 100+ pre-built toolkits. Notion, Linear, GitHub, Jira, Stripe, HubSpot, YouTube, Wikipedia, and more. Any of them can be wired into Pal.

I'd start with gmail and slack. They're already wired and just need your creds:

- **Gmail + Calendar:** Add GOOGLE_CLIENT_ID, GOOGLE_CLIENT_SECRET, and GOOGLE_PROJECT_ID.
- **Slack interface:** Add SLACK_TOKEN and SLACK_SIGNING_SECRET.

Then keep going. If a toolkit exists in Agno, you can add it to Pal. If it doesn't, you can write a custom tool in a few lines with claude code. The context graph pattern stays the same. The systems it navigates are up to you.

## What's Next

Pal is an experiment. A working one, but still an experiment. I built it to explore what a personal context-agent could look like, and I learn something new every time I use it.

Context agents can also be built for teams and organizations: an agent that navigates across Slack, Notion, Drive, and internal databases, learns which retrieval strategies work for your org's data, and gets smarter every week. That's Scout. More on that soon.

If you try Pal and something doesn't work, please share. I'm human, I make mistakes, and there are almost certainly bugs in the code. Open an issue, send me a message, or just tell me what broke. I'll fix it.

---

Pal is open source. Build on it, break it, make it yours. Links:

- [Pal on GitHub](https://github.com/agno-agi/pal)
- [Agno Docs](https://docs.agno.com/)
- [AgentOS Docs](https://docs.agno.com/agent-os/introduction)
