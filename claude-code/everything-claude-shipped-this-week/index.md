# Everything Claude Shipped This Week That Nobody Is Talking About

**Author:** Nav Toor (@heynavtoor)
**Date:** March 20, 2026
**Source:** https://x.com/heynavtoor/status/2035085093191262378
**Stats:** 11 replies, 18 reposts, 196 likes, 394 bookmarks, 28.4K views

---

Something unusual happened this week.

Between March 13 and March 20, 2026, Anthropic shipped seven major features for Claude. Not minor updates. Not bug fixes. Seven features that fundamentally change what Claude can do and how you interact with it.

Dispatch lets you control Cowork from your phone. Channels lets developers message Claude Code through Telegram and Discord. Voice mode lets you talk to Claude Code instead of typing. The 1M token context window went generally available. A double usage promotion gave everyone twice the capacity. Memory rolled out to all users. And a new command called /loop turns Claude Code into a recurring monitoring system.

Seven features. Seven days. And most people know about one or two of them at best.

I have been tracking every Claude release since January. This is, without question, the most significant product week Anthropic has ever had. Not because any single feature is revolutionary on its own. Because together, they signal a shift that most people have not processed yet: Claude is no longer a chatbot you visit. It is becoming an always-on system that works across your devices, your apps, and your schedule — whether you are watching or not.

Here is every feature, what it actually does, who it is for, and why it matters. No hype. Just the facts.

# 1. Dispatch — Control Cowork from your phone

**Launched:** March 17, 2026

**Who it is for:** Anyone using Claude Cowork (non-developers)

**What it does:** Creates one persistent conversation between the Claude mobile app on your phone and the Claude Desktop app on your computer. You send tasks from your phone. Claude runs them on your desktop. You come back to finished work.

Before Dispatch, Cowork was chained to your desk. You had to sit in front of your computer, keep the app open, and watch Claude work. Dispatch removes that requirement.

The setup takes two minutes. Open Cowork on your desktop, click Dispatch in the sidebar, scan a QR code with your phone, and you are paired. No API keys. No configuration files. Just scan and go.

What works well right now: information retrieval, file lookups, email summaries through connectors, meeting prep, and document searches. What is still inconsistent: multi-step workflows that chain several connectors together, and any task that ends with sharing or sending.

MacStories tested it and reported roughly 50/50 reliability on complex tasks. This is a research preview. But even at 50%, the ability to text your AI from bed and come back to a finished briefing is a meaningful change in how people work.

**Available to:** Max subscribers now. Pro subscribers within days.

# 2. Channels — Message Claude Code through Telegram and Discord

**Launched:** March 20, 2026

**Who it is for:** Developers using Claude Code

**What it does:** Connects your Claude Code terminal session to Telegram or Discord through an MCP plugin. You message your bot from your phone, Claude Code receives the instruction, executes it, and replies back in the chat.

VentureBeat called this the OpenClaw killer — and the comparison is fair. OpenClaw, the open-source AI agent framework that went viral earlier this year, offered similar functionality but required a dedicated Mac Mini, Node.js 22+, a WebSocket gateway, and significant technical setup. Channels requires installing a plugin and scanning a code.

The architecture is elegant. When you start Claude Code with the --channels flag, it spins up a polling service that monitors your chosen messaging platform. When a message arrives, it gets injected into your active session. Claude executes the task and replies back through the same channel.

MacStories tested it within hours of launch and was impressed. They built and ran an iOS project, deployed it wirelessly to an iPhone, compiled a list of 83 articles from Readwise Reader, and kicked off a transcription skill — all from Telegram. Their verdict: despite being a research preview, it already works.

The one limitation: if Claude Code hits a permission prompt while you are away, the session pauses until you approve locally. For fully unattended use, you can use the --dangerously-skip-permissions flag, but only in environments you trust.

**Available to:** All Claude Code users with claude.ai login. Team and Enterprise organizations must enable it in admin settings.

# 3. Double usage promotion — 2x capacity during off-peak hours

**Launched:** March 13, 2026

**Who it is for:** Everyone. Free, Pro, Max, and Team plans.

**What it does:** Doubles your Claude usage during off-peak hours, defined as any time outside 8AM to 2PM Eastern Time. Runs through March 27.

This is straightforward but easy to miss. If you use Claude outside of US morning hours, you get twice as much capacity. No signup. No coupon code. It just works automatically.

The geographic math matters. If you are in India, off-peak hours translate to roughly 6:30 PM to 12:30 AM IST — covering your entire evening work session. If you are in Asia-Pacific, off-peak covers virtually your entire working day. If you are on the US East Coast, you benefit for roughly half of your workday.

The bonus usage does not count toward your weekly rate limits. This is genuinely free extra capacity, not a reshuffling of existing limits.

Why it matters beyond the obvious: this promotion is likely Anthropic's first experiment with time-based pricing. Flat-rate, all-you-can-eat pricing for AI services was always going to be temporary. The compute costs are too high. If this promotion works, expect more dynamic pricing in the future.

**Runs through:** March 27, 2026. Enterprise plans are excluded.

# 4. 1M token context window — Now generally available

**Launched:** March 16, 2026

**Who it is for:** All Claude Platform users, Claude Code Max/Team/Enterprise users

**What it does:** Opus 4.6 and Sonnet 4.6 now include the full 1 million token context window at standard pricing. No multiplier. No premium tier.

To put this in perspective: 1 million tokens is roughly 750,000 words. That is about ten full-length novels. Or an entire codebase. Or every email you have sent and received in the past year.

Before this week, the 1M context window was in beta with limited access. Now it is standard. And the pricing change matters: there is no cost multiplier for using the full window. You pay the same rate whether you use 10,000 tokens or 1,000,000.

For Cowork users, this means fewer compactions. Compaction is what happens when your conversation gets too long and Claude has to summarize earlier parts to free up space. With 1M context, entire working sessions can fit without compaction. Your instructions from the beginning of the session are still fully accessible at the end.

For Claude Code users, this means entire repositories can be loaded into a single session. Debugging across dozens of files becomes one continuous conversation instead of a fragmented series of handoffs.

**Available on:** Claude Platform (API), Claude Code for Max, Team, and Enterprise users with Opus 4.6.

# 5. Voice mode — Talk to Claude Code instead of typing

**Rolling out:** March 2026 (currently ~5% of users)

**Who it is for:** Claude Code users

**What it does:** Push-to-talk voice input for Claude Code. Hold spacebar to speak. Release to send. Claude transcribes and processes your instruction.

This is not an always-listening system. It is a controlled, push-to-talk mechanism. You hold down the spacebar (or a custom key you configure), speak your instruction, and release. Claude transcribes it and treats it like any typed input.

The transcription supports 20 languages as of this week, including English, Spanish, French, Chinese, Japanese, Portuguese, German, Russian, Polish, Turkish, Dutch, Ukrainian, Greek, Czech, Danish, Swedish, and Norwegian. The system has been optimized for technical terms and repository names, which is the detail that matters most for developers.

Voice mode is activated with the /voice command. Many developers report that they can dictate complex requirements faster than typing them, especially for explaining multi-step workflows or describing bugs.

The rollout is gradual. If you do not see it yet, update Claude Code to the latest version and check again in a few days. Anthropic is expanding access progressively.

**Current availability:** Approximately 5% of Claude Code users. Rolling out over the coming weeks.

# 6. Memory for all users — Claude now remembers you

**Launched:** Early March 2026

**Who it is for:** All Claude users, including free users

**What it does:** Claude retains context and preferences across conversations. Your name, your writing style, your ongoing projects, your preferences — all persist between sessions.

Until this month, every conversation with Claude started from zero. No memory of previous discussions. No retained preferences. No context from past work. You re-explained yourself every single session.

Memory changes that. Claude can now remember who you are, what you are working on, how you like your responses formatted, and what topics you have discussed before. It uses this context automatically in new conversations.

For Cowork users who already built context files (about-me.md, brand-voice.md, working-style.md), memory adds another layer. Your context files handle the deep, structured knowledge. Memory handles the conversational continuity between sessions — the small preferences and ongoing threads that would be tedious to encode in files.

You can import your ChatGPT memory settings directly into Claude with one click. For anyone switching from ChatGPT during the current migration wave, this removes one of the biggest friction points.

You can view and edit what Claude remembers about you in Settings. Nothing is hidden. You control what stays and what gets removed.

**Available to:** All users, including free plans.

# 7. /loop — Recurring tasks inside Claude Code

**Launched:** March 2026 (v2.1.71)

**Who it is for:** Claude Code users

**What it does:** Define an interval and a prompt, and Claude executes it automatically on that schedule. A lightweight, session-level cron job.

The syntax is simple: /loop 5m check the deploy. That command tells Claude to check the deployment status every five minutes. It runs as long as the session is open.

Use cases that are already working: CI/CD monitoring during deployments, watching log files for specific errors, checking API endpoints at regular intervals, monitoring build status, and running periodic code quality checks.

This is not a full scheduling system. It runs within the current session and stops when you close it. For persistent scheduled tasks, Cowork's scheduled tasks feature is the better fit. But for temporary monitoring during active work, /loop fills a gap that previously required separate tooling.

**Available to:** All Claude Code users on v2.1.71 or later.

# Why this week matters more than any individual feature

Look at these seven features together and a pattern emerges.

Dispatch: control AI from your phone. Channels: control AI from Telegram and Discord. Voice mode: control AI with your voice. 1M context: AI remembers your entire working session. Memory: AI remembers you across sessions. /loop: AI runs tasks on a schedule. Double usage: more capacity to do all of it.

Every single feature this week points in the same direction: Claude is becoming less of a tool you use and more of a system that works on your behalf.

Six months ago, using Claude meant opening a browser tab, typing a prompt, reading a response, and closing the tab. Today, Claude reads your files, connects to your apps, runs on a schedule, remembers who you are, takes instructions from your phone, responds through Telegram, and listens to your voice.

That is not a chatbot anymore. That is infrastructure.

And the pace is accelerating. Cowork launched in January. Connectors and plugins launched in February. Dispatch, Channels, Voice, 1M context, Memory, /loop, and double usage all shipped in March. Each month is bigger than the last.

Claude has over one million new signups per day right now. It overtook ChatGPT as the number one app in the Apple App Store. Daily active users have more than tripled since January. The ecosystem is growing faster than most people realize.

The people who understand what happened this week are not just getting access to better features. They are getting early access to a new way of working. A way where AI is not something you visit. It is something that runs.

# What to do right now

You do not need to use all seven features. Pick the ones that match how you work.

**If you use Cowork:** Set up Dispatch. Update Claude Desktop, click Dispatch, scan the QR code, and start sending tasks from your phone. Even at research preview reliability, the morning briefing workflow alone is worth the two-minute setup.

**If you use Claude Code:** Try Channels with Telegram or Discord. Install the plugin, configure your bot, restart with --channels, and pair your phone. If voice mode is available to you, activate it with /voice and try dictating your next complex requirement.

**If you use Claude on any plan:** Use the double usage promotion before March 27. Plan your heavy Claude work for off-peak hours (outside 8AM to 2PM Eastern) and get twice the capacity for free.

**If you are on any plan including free:** Check your Memory settings. Go to Settings and see what Claude has learned about you. Edit anything that is wrong. Add anything that is missing. The more accurate your memory, the better every future conversation gets.

**If you have not tried Claude yet:** This is the week to start. Download Claude Desktop from claude.com/download. The free plan now includes memory, inline visualizations, and access to the double usage promotion. There has never been a lower barrier to entry.

Seven features in seven days. The window to learn them before everyone else catches up is right now.

It will not stay this quiet for long.
