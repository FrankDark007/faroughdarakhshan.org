---
title: "How I run Claude Code and Codex in a one-person office"
description: "Farough Darakhshan's working practices for Claude Code and Codex in a small business: which agent gets which job, local models, code search, and cost limits."
url: "/claude-code-codex-one-person-office/"
---

I'm Farough (Frank) Darakhshan. My day job is water-damage restoration paperwork: writing final invoices, defending them against adjuster cuts, and tracking 500 to 600 claims at a time until they are paid. That is the work of a department, done mostly by one person. AI coding agents are how I build the tools that make it possible.

These are the practices I settled on. They are not theory. Each one came from a week where something cost too much or broke.

## Two agents, two jobs

I use two coding agents: Claude Code and OpenAI's Codex.

For a while Claude did everything. In the last week of September 2026 my Claude weekly plan hit 95 to 96% while my Codex plan sat at 17%. I was paying for capacity I wasn't using.

Now Codex takes builds. Claude plans, reviews, and handles work that needs judgment across many files. A build goes to Codex when it can be written as a closed brief.

## A build brief has four parts

Every brief I hand an agent has the same shape:

1. **One goal.** Not "improve the invoice tool." One outcome.
2. **The files it owns.** Anything else is read-only.
3. **Hard rules.** What it must never do, such as touching customer data or production.
4. **The deliverable.** What finished looks like, and how I will check it.

An agent cannot ask follow-up questions halfway through. A thin brief comes back as a confident wrong answer.

## Background runs need a long timeout

I run Codex builds in the background. On 10/01/2026 two builds were killed mid-run, one with a 60-minute budget and one with 120. Neither had failed. The shell they ran in had a default 30-minute limit, and when it fired it took Codex down with it.

Now every background build gets a two-hour timeout, and every brief is budgeted at 110 minutes or less, so a run finishes inside the window.

## Keep configuration on the command, not the machine

Codex reads its settings from a config folder. I keep several: the default, one pointed at my local models, and several for test harnesses.

The rule is to choose the folder on the one command that needs it, never machine wide. I learned this when a global setting reached every app on the Mac, including the ChatGPT desktop app, and my projects and chats vanished from its sidebar.

## Local models for the work that can wait

I run local models on three AMD Strix Halo machines. A separate Codex profile points at that local server, so Codex can work against my own hardware at no per-token cost.

Two limits apply:

- Not every local model works with Codex. The newer Qwen models I run there do not, so Codex gets the coding model.
- Local agent transcripts are saved on the Mac. Sensitive documents such as finance and tax files go through the private chat interface on the box itself, not through an agent.

## Give every agent real code search

Agents waste a lot of effort reading whole files to find one function. I give every Claude Code and Codex agent the same symbol-search tool, Serena, backed by language servers.

I first ran it on an editor backend. It only worked in repos that editor happened to have open, so headless agents got nothing. Switching to language servers fixed that for every agent.

One hook reminds agents to use it. Unwrapped, the hook also fired outside any repo and blocked an ordinary search in my home folder, so it now runs only inside a repository. On 10/20/2026 I confirmed real Claude Code and Codex runs actually called it.

## Context is the bill

The biggest cost lesson was context size. In the week of 9/23 to 9/30, 65% of all the context tokens I used were on calls above 200,000 tokens. Sessions ran to about 467,000 before they compacted. The part above 200,000 cost about $1,100 that week at list price, about 21% of the total.

So Claude Code now compacts at 200,000 tokens, with a reminder to save work before it does. Codex compacts at 120,000.

## Rules for helper agents

Claude Code can spawn helper agents. They are useful and expensive. My rules:

- **Helpers run on the smaller model by default.** I measured bigger-model helpers at 18% of a week's usage. Defaulting to the smaller model saves about 9%.
- **No helper for small work.** If it takes about five steps, the main session does it. A helper costs a full context window to start.
- **No helper for checking.** Verification happens in the session that did the work.
- **One writer per file or folder.** Two agents editing the same thing is how work gets lost.

## What this adds up to

None of this is clever. It is a short list of limits that let one person keep several agents busy without paying for waste or losing work. The tools will keep changing. These limits are what I check first.

My public code is on [the index](/). Longer write-ups are on my [working notes](https://faroughdarakhshan.us/), and the systems themselves are on my [projects page](https://faroughdarakhshan.com/projects/).
