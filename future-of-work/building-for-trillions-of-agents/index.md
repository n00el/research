---
title: "Building for trillions of agents"
author: "Aaron Levie"
handle: "@levie"
date: 2026-03-08
source: https://x.com/levie/status/2030714592238956960
category: future-of-work
likes: 2561
retweets: 354
replies: 140
views: 596750
bookmarks: 4834
---

# Building for trillions of agents

**Author:** Aaron Levie ([@levie](https://x.com/levie)) — CEO @Box
**Date:** March 8, 2026
**Source:** [X / Twitter](https://x.com/levie/status/2030714592238956960)

**Stats:** 2,561 likes | 354 retweets | 140 replies | 596,750 views | 4,834 bookmarks

---

![Cover image](cover.jpg)

---

Over the past few months, something big has started to happen with agents. Near the tail end of last year, we began to reach the point where coding agents were able to complete much longer running tasks, and didn't require as much hand-holding throughout the development process.

These agents are no longer chatbots with basic tools. Instead, these agents often have their own sandboxed compute environment, have the ability to write and run code for any problem they run into, interact with APIs and CLIs directly, have their own file systems and long-term memory, and so on. This set of core primitives, general progress on agentic harness best practices, and insane model progress on agentic tool-use and software development, offer a glimpse of agents that can handle any task that can be thrown at them.

And while this architecture was initially defined by a mix of coding agents like Claude Code, Devin, Codex, Factory, Cursor, or Replit, we've recently crossed the chasm into all areas of personal experiences and knowledge work now with agents like Claude Cowork, Perplexity Computer, Manus, and of course OpenClaw, which took things far further into the future, with an agent that runs 24/7 in its own persistent environment.

Because of rapidly advancing capabilities, agents will be brought into nearly all areas of work. Agents will be deployed to review every contract that gets written, handle the front lines of most customer support cases, audit every company's financials, comb through every piece of medical research for drug discovery, generate nearly all code that ever gets written, create most sales and consulting presentations, transact across the web for consumers, and overall, be involved in nearly every other economically valuable task in society.

And it won't just be about executing the tasks that we already do today. We'll use agents to do vastly more than we did before — we'll use agents to run simulations that would have been unaffordable before, we'll use them to prototype every idea we have with many different options, we'll pursue vastly more projects because it's cheap to get going and easy to shut down, and we'll review every piece of data vs. sampling information.

When you add this all up, we can expect that nearly every employee in an organization will have many agents working on their behalf, and it isn't hard to imagine an enterprise with 100X or 1,000X more agents than people in a company. As a result of trillions of agents running around, agents will become the primary user of all software in the future.

Given most software was built for people to use, this means we're going to see a major shift in what the future of software looks like. So what's next?

## Make something agents want

Paul Graham famously framed how to build software in the simplest possible terms: make something people want.

This advice led to some of the biggest software success stories of the 21st century, and helped lead to a movement for building tools that were simple to use, easy to adopt, solved clear problems without jargon, had straightforward pricing, and more.

Now, the path forward is to make software that agents want. While the biggest users of agents tend to be developers or at least highly technical users that often will have their own preferences of tools, in a world of agents doing any type of task for knowledge workers, this type of preference will slowly drift away. Short of an enterprise already having a standard, agents will then be in the driver's seat for what gets adopted for any particular workflow.

This could mean the tools they sign up for, the code that they write, the libraries that they use, the skills they leverage, and so on. The platforms that are easier for agents to adopt, and solve the agent (and user's) problems the best, will get ahead far faster than those that don't. Agents won't be going to your webinar or seeing your ad; they're just going to use the best tool for the job, and you'll want it to be yours.

The biggest implication of this advice is that everything must become API-first in what you're building. If you don't have an API for a feature, it might as well not exist. If it can't be exposed through a CLI or MCP server, you're at a disadvantage. If you have confusing APIs and conflicting paths for an agent to pursue, you're just compromising your chances of being useful for agents. At Box, as we focus on building a file system for agents, we've been combing over every aspect of our API to figure out what breaks down in a world of agents for a level of usability that normally has only gone into UX design.

Just as designing for users meant putting yourself in their shoes when building software, the same is true when thinking about what agents will run into. For instance, Jared Friedman at YCombinator put everyone on notice: *"Even the best developer tools mostly still don't let you sign up for an account via API. This is a big miss in the claude code age because it means that claude can't sign up on its own. Putting all your account management functions in your API should be tablestakes now."* If an agent can't easily sign up for your service and starting using it, you're basically dead to agents.

There are also major business model implications to a world where agents are the biggest users of software in the future. In some cases user seats kicking off agents may comfortable fit into a seat-based business model for software, but there are plenty of use cases for agents that don't attach neatly to an existing user or because their volume of workload is now totally different. For instance, with a few words or a couple lines of text, an agent might do hours of human-equivalent work within software, and just expose the final output to the end-user.

This will ultimately mean an evolution of business models for some parts of software, as any tool that wants to survive an agentic future needs some form of consumption or volume-based business model built into their system, even supporting agents being able to make their own payments for these services.

## The next era of infra and tools for agents

> *"it was a good idea to give computers to humans. it's a better idea to give computers to computers so that they can create the same outputs we do on a computer for our work."* — Aravind Srinivas, Perplexity

As agents have their own computers they use, and can write and execute their own code, call in oft-used skills for repeated actions, as well as tap into external tools and services, this creates an opportunity for an all new set of technologies for agents to use. Just imagine what a user does on a computer, and an agent will need a similar set of capabilities designed just for them.

Some of these core services will naturally come from existing players, because the agent is tapping into existing data, or there is value to the collaboration or connection between the existing human user and the agentic user on a system. Equally, there are going to be all new categories that emerge because the problem space is just *so* different from what a human user needed or could do before, that it makes sense to design the service from the ground up.

For instance, it's clear agents will need to have their own infrastructure to operate with, and at a scale that we've never seen before. The next hyperscaler (or an existing one) will be built on the back of the idea that the future server farms won't be used for our applications, but instead, our agents. E2B, Daytona, Modal, and Cloudflare are all pushing in this direction, and these sandboxed environments will rival any scale of computing we've ever seen before.

Agents will also need access to core files in an enterprise, as well as be able to manage their own data for memory and long running work, which is what we're focused on building at Box. Similarly, major enterprise systems will need to become API-first for enabling agents to work with critical services and data from an organization — such as HRIS, CRM, workflows, data lakes, and other major systems. The products that offer the most seamless tools for agents to operate on this data from anywhere will be in the best position to win over these future workloads.

Agents will likely also need identities, and have the ability to communicate with others; Agentmail, for instance, is providing mailboxes for agents to have their own persistent email to work with. Parallel, Exa, and others are rebuilding web search for a world in which agents are the biggest users crawling the web for information. Many types of agents will need to manage their own budgets of what they can spend on with wallets from Stripe or Coinbase, and we may finally get a real use case for microtransactions, where agents can tap into paywalled tools and information.

Security, compliance, and governance will become a major problem for these agents. In a world where agents are accessing and working on sensitive information in a workflow, or where they're executing regulated workflows (like in pharma or banking), companies will need to govern and retain all the work that these agents did. Long-running agents will likely need their own identities that allow them to authenticate into services, offering strict controls on what types of actions they can take and what data they have access to in an enterprises. We'll need all new software and platforms to help with these challenges, just like we've built out for people and applications over time.

Overall, we are clearly entering a new era of software where we need to design and build our tools specifically for agents to use at scale. In a world of trillions of agents doing work, this will usher in a completely new way of working with software.
