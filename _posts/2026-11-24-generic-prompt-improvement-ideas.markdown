---
layout: post
title: "Generic Prompt Improvement Ideas"
date: 2026-11-24
description: "A product agnostic set of prompt improvements for getting more out of LLM coding agents, distilled from a 28 minute talk on practical prompting."
img: prompt_improvement/header.jpg
tags: [ai, llm, prompting, coding-agents]
---

## Original Post

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">This is f*cking unreal.
The engineer who built Claude Code just dropped a 28-minute video on how to
write prompts that actually work.

I've seen $600 courses that don't cover what he shows in the first 5 minutes.

Watch it, then read the guide below on the Claude features 99% of users never
find.</p>&mdash; Anatoli Kopadze (@AnatoliKopadze) <a
href="https://x.com/AnatoliKopadze/status/2122348438603825649?ref_src=twsrc%5Etfw">November
16, 2026</a></blockquote>
<script async src="https://platform.x.com/widgets.js" charset="utf-8"></script>

## Motivation

The video above caught my eye through the post, and it delivered on the hype. It is a 28 minute talk from an engineer at Anthropic on how to write prompts that actually work with coding agents. The part that sold me, though, is that the techniques are not tied to the product. Every trick in the video is about how you prompt, not which assistant you use. That made it worth distilling into a list of generic ideas that you can apply to any LLM you drive in an agentic loop, whether it is a terminal assistant, an IDE agent, or your own SDK integration.

Where the talk names a product feature, such as a memory file, an MCP server, a slash command, or an `@file` mention, I have substituted the equivalent mechanism in my own stack. The goal is to keep the advice portable.

One number from the talk made the point for me. Anthropic's technical onboarding dropped from two to three weeks down to two to three days once new hires started with codebase Q&A instead of jumping straight into feature work. Better prompting, in this case, is better onboarding.

## Generic Prompt Improvements

The short version:

1. Start with codebase Q&A before asking for any edits.
2. Make the model plan, and get your approval, before it writes code.
3. Teach it your tools, and persist the usage notes.
4. Tailor the workflow to the task, and give it a way to check its own work.
5. Feed it persistent context, and keep that context short.
6. Tune the context deliberately, and share it with your team.

### 1. Start with codebase Q&A

The top recommendation is to begin with code based questions, not code edits.

- Ask the way you would ask a colleague: how is this piece of code used? How do I instantiate this thing? Why does this function take 15 arguments, and why are they named that way? Which version released this API? What did I ship last week?
- Point at specifics: file paths, issue and pull request numbers, git history. A capable agent goes a level deeper than text search. It finds real examples of how a class is instantiated, walks the commits to see why an argument was introduced and which issue it was tied to, and fetches the linked issue for context.
- Do not over-explain the mechanics. "Look through git history to answer" is enough, because the model already knows git. You supply the what, not the how.
- Q&A is also your calibration tool. It teaches you the model's actual boundary: what it can one-shot, what needs two or three shots, and where you need to hold its hand.

Here are some example prompts from the talk that you can adapt for your own codebase:

```text
How is src/orders/OrderController.py used?

How do I make a new src/app/services/ValidationTemplateFactory?

Why does recoverFromException take so many arguments? Look through git history to answer

Why did we fix issue #18363 by adding the if/else in src/login.ts API?

In which version did we release the new api/ext/PreHooks.php API?

Look at PR #9383, then carefully verify which app versions were impacted

What did I ship last week?
```

### 2. Make it plan before it writes code

If you ask an agent for an enormous feature in one shot, it sometimes gets it right, and sometimes builds something that is not at all what you wanted. The cheapest fix is to make it think first: brainstorm ideas, produce a plan, and ask for your approval before it writes code. No special mode or flag is needed. You simply ask, "Before you write code, make a plan."

- Keep a human in the loop: "Propose a few fixes for issue #8732, then implement the one I pick."
- Shorthand works too. "commit, push, pr" is a common incantation. A competent model creates the branch, reads the git log to match your commit style, pushes, and opens the pull request, with nothing in the system prompt telling it to.
- For genuinely hard problems, explicitly request more reasoning. Several models respond to keywords such as "think hard" or "ultrathink".

### 3. Teach the agent to use your tools

An agent is only as useful as the tools it can reach. Two kinds of tools are worth wiring in:

- Bash/CLI tools your team already uses. The prompting pattern is to name the tool and the job: "Use the barley CLI to check for error logs in the last training run. Use -h to check how to use it." The model runs `-h` and learns the interface by itself.
- MCP (Model Context Protocol) servers. Register the server, then say "Use the X MCP server to check for error logs in the last training run." It will start using it immediately. If your stack has no MCP, the equivalent is any documented tool the agent can call: a script, an API wrapper, or a database client.

If a tool is part of your daily work, write the usage notes into your persistent memory file so the agent remembers them across sessions instead of re-learning them every time. Starting on a new codebase? Hand the agent every tool your team already uses for that codebase. It will figure out how to string them together with its built-in file, search, and shell tools.

### 4. Tailor the workflow to the task

Pick the shape of the workflow that fits the task. Three workflows cover most day to day work:

1. **Explore, plan, confirm, code, commit**, for bugs and anything risky. "Figure out the root cause for issue #983, then propose a few fixes. Let me choose an approach before you code."
2. **Write tests, commit, code, iterate, commit**, for behaviour changes. "Write tests for `src/utils/markdown.ts` to make sure links render properly (note the tests won't pass yet, since links aren't implemented). Then commit. Then update the code to make the tests pass."
3. **Write code, screenshot, iterate**, for UI work. "Implement this mock. Then screenshot it with a headless browser and iterate till it looks like the mock." Good agents are multimodal: drag a mock image in and have it implement against it.

The underlying trick: give the model any channel to check its own work, unit tests, integration tests, a browser or simulator screenshot, and it will iterate by itself. The first pass gets it pretty good. Two or three feedback loops get it almost perfect.

### 5. The more context you give, the smarter it gets

You carry a lot of institutional knowledge, architecture, conventions, history, that the model cannot see. The more of it you make visible, the better its decisions get. A useful hierarchy for where that context lives:

- **Project root memory file.** Checked in, shared with the team, read at the start of every session.
- **Project local memory file.** Not checked in, for personal preferences.
- **User level memory file.** Applies across all of your projects.
- **Enterprise level memory file.** Rolled out to every developer.
- **Nested per directory memory files.** Pulled in on demand when the agent works in that directory.

Put in the things you would need to know to work in the codebase: common shell commands, the style guide, core files, architectural decisions, and how to use your tools. Keep it short. A memory file that is too long burns context window for little gain.

A few extra ways to pull in context:

- Tell the agent what to remember in the moment, and let it write the note into the right memory file for you.
- Mention files directly in the prompt instead of pasting their contents.
- Check custom commands or scripts into the repo so a single invocation runs a whole workflow.

The same hierarchy extends beyond memory to permissions and tool configuration. Shared project context is the highest leverage place to start: write it once, and the whole team benefits.

### 6. Take the time to tune your context

Context is not free, and not all context is equal. Before adding anything, ask: does this belong in every session, or only on demand? Is it for the team, or just for me?

Run important prompts and context blocks through a "prompt improver" pass. Yes, use the model to rewrite your own instructions. The talk is blunt that this pays off: done right, tuning your context improves performance dramatically.

### Quick wins

- **Learn the interaction shortcuts.** A small set of key bindings changes the feel of every session: an accept edits mode for when you trust the direction, a bash escape hatch whose output still lands in the context window, a stop key that is always safe to press mid action, history navigation, full output expansion, and session resume so long work can be picked up later.
- **Pipe it like a Unix utility.** Non interactive print mode turns the agent into a filter: pipe data in, get structured output out. `git status | agent -p "what are my changes?"` The combinations are essentially endless, from CI to incident response to triaging giant logs.
- **Run several agents in parallel.** Power users rarely run one agent at a time: separate terminal or `tmux` sessions, `git worktrees` or separate checkouts for isolation, and CI runners for unattended jobs. A surprising amount of work gets done in parallel.

### The through-line

Treat the model like a fast, well read junior colleague: point it at the right files, tell it what good looks like, give it a way to check its own work, and write down what it should always know. Do those four things with any LLM, and the results compound.

***

### References
- Distilled from a talk by Boris (Anthropic) on practical prompting with coding agents (video and transcript).
- Original post by Anatoli Kopadze: [x.com/AnatoliKopadze](https://x.com/AnatoliKopadze/status/2122348438603825649).
