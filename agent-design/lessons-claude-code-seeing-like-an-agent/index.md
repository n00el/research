# Lessons from Building Claude Code: Seeing like an Agent

**Author:** Thariq Shihipar (@trq212)
**Date:** February 27, 2026
**Source:** https://x.com/trq212/status/2027463795355095314
**Stats:** 1,952 likes, 48 replies

---

![Cover image](image-1.jpg)

## Constructing the Action Space

One of the hardest parts of building an agent harness is constructing its action space. Claude acts through Tool Calling, and there are a number of ways tools can be constructed in the Claude API with primitives like bash, skills, and code execution. Given all these options, the question becomes: how do you design the tools of your agent? Do you need just one tool like code execution or bash? What if you had 50 tools, one for each use case?

To put myself in the mind of the model I like to imagine being given a difficult math problem. What tools would you want in order to solve it? It would depend on your own skills! Paper would be the minimum, but you'd be limited by manual calculations. A calculator would be better, but you would need to know how to operate the more advanced options. The fastest and most powerful option would be a computer, but you would have to know how to use it to write and execute code.

This is a useful framework for designing your agent -- you want to give it tools that are shaped to its own abilities. But how do you know what those abilities are? You pay attention, read its outputs, experiment. You learn to see like an agent.

Claude Code currently has ~20 tools, and we are constantly asking ourselves if we need all of them. The bar to add a new tool is high, because this gives the model one more option to think about.

## The AskUserQuestion Tool

When building the AskUserQuestion tool, the goal was to improve Claude's ability to ask questions (elicitation). While Claude could just ask questions in plain text, answering those questions felt like they took an unnecessary amount of time. The goal was to lower friction and increase communication bandwidth between the user and Claude.

The first thing we tried was adding a parameter to the ExitPlanTool to have an array of questions alongside the plan. This was the easiest thing to implement, but it confused Claude because we were simultaneously asking for a plan and a set of questions about the plan. What if the user's answers conflicted with what the plan said? Would Claude need to call the ExitPlanTool twice? We needed another approach.

Next we tried modifying Claude's output instructions to serve a slightly modified markdown format that it could use to ask questions -- for example, outputting a list of bullet point questions with alternatives in brackets, which could be parsed and formatted as UI for the user. While Claude seemed okay at outputting this, it was not guaranteed. Claude would append extra sentences, omit options, or use a different format altogether.

Finally, we built a dedicated tool that Claude could call at any point, particularly during plan mode. When triggered, it would show a modal to display questions and block the agent's loop until the user answered. The tool allowed us to prompt Claude for a structured output and helped ensure Claude gave the user multiple options. It also gave users ways to compose this functionality.

Most importantly, Claude seemed to like calling this tool and the outputs worked well. Even the best designed tool doesn't work if Claude doesn't understand how to call it.

## The TodoWrite Tool and the Task Tool

When Claude Code first launched, we realized that the model needed a Todo list to keep it on track. Todos could be written at the start and checked off as the model did work. To do this we gave Claude the TodoWrite tool, which would write or update Todos and display them to the user.

But Claude often forgot what it had to do, so we inserted system reminders every 5 turns to remind Claude of its goal. As models improved, they not only did not need to be reminded of the Todo List but could find it limiting -- being sent reminders of the Todo List made Claude think it had to stick to the list instead of modifying it.

We also saw Opus 4.5 get much better at using subagents, which prompted the question of how subagents could coordinate on a shared Todo List. This led us to replace TodoWrite with the Task Tool.

Whereas Todos were about keeping the model on track, Tasks were more about helping agents communicate with each other. Tasks could include dependencies, share updates across subagents, and the model could alter and delete them.

As model capabilities increase, the tools that your models once needed might now be constraining them. It's important to constantly revisit previous assumptions on what tools are needed.

## Search Tools and Context Building

A particularly important set of tools for Claude are the search tools used to build its own context. When Claude Code first came out, we used a RAG vector database to find context for Claude. While RAG was powerful and fast, it required indexing and setup and could be fragile across different environments. More importantly, Claude was given this context instead of finding it itself.

By giving Claude a Grep tool, Claude could search for files and build context itself. As Claude gets smarter, it becomes increasingly good at building its context if given the right tools. Over the course of a year, Claude went from not really being able to build its own context to being able to do nested search across several layers of files to find the exact context it needed.

## Progressive Disclosure

When we introduced Agent Skills, we formalized the idea of progressive disclosure, which allows agents to incrementally discover relevant context through exploration. Claude could read skill files and those files could then reference other files the model could read recursively. A common use of skills is to add more search capabilities -- like instructions on how to use an API or query a database.

Progressive disclosure is now a common technique we use to add new functionality without adding a tool.

We noticed that Claude did not know enough about how to use Claude Code itself -- for example, if you asked it how to add a MCP or what a slash command did, it would not be able to reply. We could have put all of this information in the system prompt, but given that users rarely asked about this, it would have added context rot and interfered with Claude Code's main job: writing code.

Instead, we tried a form of progressive disclosure -- we gave Claude a link to its docs which it could then load to search for more information. This worked, but we found that Claude would load a lot of results into context to find the right answer when really all you needed was the answer.

So we built the Claude Code Guide subagent, which Claude is prompted to call when you ask about itself; the subagent has extensive instructions on how to search docs well and what to return. We were able to add things to Claude's action space without adding a tool.

## Conclusion

If you were hoping for a set of rigid rules on how to build your tools, unfortunately that is not this guide. Designing the tools for your models is as much an art as it is a science. It depends heavily on the model you're using, the goal of the agent and the environment it's operating in. Experiment often, read your outputs, try new things. See like an agent.
