---
layout: post
title:  "Work in the Era of AI — Two Paths to Your AI-Powered Future"
date:   2026-05-12 11:00:00 +0000
categories: AI Copilot MCP WorkIQ Productivity
---

Hello again, world! It's been four years since my last post. Back then I was containerizing apps and showing off Surface Duos. A lot has changed — I've moved through Business Applications, Modern Work, and now sit squarely in AI Business Solutions. But more importantly, the *world* has changed. AI went from a buzzword in conference keynotes to the coworker who never sleeps.

So I'm back, and the theme going forward isn't code tutorials (the coding agents are honestly better at those than I am 😄). Instead, I want to explore how AI is reshaping the way we work, how businesses evolve around it, and what it all means for the people in between.

Let's dive in.

## The Upward Spiral

Here's what fascinates me: technology and work are in a constant feedback loop. AI changes how we work, and how we work shapes the next wave of AI. Each cycle lifts the bar higher. A year ago, AI could summarize your meeting notes. Today, it can run a multi-step workflow across your email, calendar, and documents — autonomously — while you grab coffee.

That's not incremental improvement. That's a phase shift.

## M365 Copilot: Where We Are Now

Microsoft 365 Copilot has entered what they call **Wave 3** — and it's a big one. Agentic capabilities are now generally available across Word, Excel, and PowerPoint. The [2026 Work Trend Index](https://www.microsoft.com/en-us/microsoft-365/blog/2026/05/05/microsoft-365-copilot-human-agency-and-the-opportunity-for-every-organization/) reports that 58% of AI users say they're producing work they *could not have done* a year ago. Among power users? That number jumps to 80%.

But the real headline is something far more exciting.

## Copilot Cowork: Your AI Just Got Promoted

On May 5th, Microsoft announced [Copilot Cowork](https://www.microsoft.com/en-us/microsoft-365/blog/2026/05/05/copilot-cowork-from-conversation-to-action-across-skills-integrations-and-devices/) — and it fundamentally changes the relationship between you and your AI assistant. Here's the simplest way I can put it:

> **Old Copilot:** "Here's what you could do."  
> **Cowork:** "I'll go do it."

![IMAGE: Split comparison illustration — left side shows a person typing questions into a chat bubble labeled 'Ask', right side shows an AI agent juggling multiple tasks like email, calendar, and documents labeled 'Cowork', connected by an upward arrow showing evolution]({{site.baseurl }}/assets/blogs/05122026_pic01.png)

Cowork handles multi-step, cross-app workflows that run for minutes or even hours. Draft and send a stakeholder update email, reorganize your project files, prep a meeting brief with relevant docs — all from a single natural language request.

And here's where it gets really interesting: **Skills**.

## Skills: Teaching Your AI How *You* Work

[Cowork's Skills system](https://learn.microsoft.com/en-us/microsoft-365/copilot/cowork/) lets you create reusable instruction sets that tell the AI *how* to complete specific tasks. Drop a `SKILL.md` file in your OneDrive, describe the workflow, and Cowork picks it up automatically. Up to 50 custom skills per user. It's like writing a playbook for your tireless digital coworker.

Skills in Cowork are essentially natural language instruction sets — think of them as playbooks written in plain English that tell the AI *how* to approach a task.

Meanwhile, there's a parallel movement happening in the broader AI ecosystem: the **Model Context Protocol (MCP)** — an open standard originally proposed by Anthropic and now MIT-licensed. Think of MCP as the **USB-C for AI**: one standard interface that any AI host can use to connect to any compatible tool or data source. It's been adopted across the industry — Claude, ChatGPT, VS Code, GitHub Copilot, Cursor — with official SDKs in ten programming languages.

Skills and MCP are complementary ideas. Skills define *what* to do and *how* to do it. MCP defines *how to connect* — giving AI agents standardized access to tools, data, and external systems. Together, they represent a powerful shift: we're moving from telling AI things one conversation at a time to building reusable, portable, machine-readable knowledge about how work gets done.

![IMAGE: A clean hub-and-spoke diagram with 'MCP' in the center, surrounded by connected nodes labeled Claude, ChatGPT, VS Code, GitHub Copilot, Cursor, and Custom Tools — illustrating the universal protocol connecting AI platforms]({{site.baseurl }}/assets/blogs/05122026_pic02.png)

## But Wait — Skills + AI ≠ Magic (Yet)

A powerful AI model with great skills is impressive. But drop it into your work without context and watch it fumble. It's like hiring a brilliant new employee on their first day — smart, capable, but clueless about how *your* team actually operates.

AI needs **your context**: your work patterns, your collaborators, your priorities, where the data sits across your email, chat, document libraries, and meetings. And not just the raw data — the *intelligence* on top of it.

Here's a concrete example. You ask Copilot: *"Prep me for Monday's meeting with Contoso."*

**Without context**, you get a calendar invite summary. Helpful, but shallow.

**With context**, the AI pulls your last three email threads with their team, the shared proposal you edited on Friday, the Teams chat where your colleague flagged a pricing concern, and your notes from last quarter's QBR. Then it synthesizes a briefing tailored to what *you* specifically need to walk in prepared.

That's a night-and-day difference. And that's exactly what **WorkIQ** delivers.

## WorkIQ: The Brain Behind the Brawn

Introduced in March 2026, [WorkIQ](https://techcommunity.microsoft.com/blog/microsoft365copilotblog/a-closer-look-at-work-iq/4499789) is the intelligence layer that makes Copilot genuinely *context-aware*. If you're familiar with Microsoft Graph, here's an analogy:

> **Microsoft Graph** is the plumbing — pipes and faucets connecting you to M365 data.  
> **WorkIQ** is the intelligence that knows *which* faucet to turn, *when*, and *why*.

![IMAGE: A layered pyramid diagram with three tiers — bottom tier labeled 'Data' showing icons for email, calendar, files, Teams; middle tier labeled 'Context' showing a brain icon with semantic index and memory; top tier labeled 'Skills and Tools' showing gear and plugin icons — with 'WorkIQ' written along the side spanning all three]({{site.baseurl }}/assets/blogs/05122026_pic03.png)

WorkIQ operates across three layers: **Data** (your M365 tenant, Dynamics 365, external connectors), **Context** (semantic indexing, explicit and implicit memory that learns your patterns over time), and **Skills & Tools** (the agentic capabilities that act on your behalf).

This is genuinely a new chapter for personal productivity.

## The Honest Conversation: What It Can't Do (Yet)

Now, I wouldn't be writing a useful blog if I didn't keep it real. (Side note: yes, this post is AI-*assisted*. I'm practicing what I preach. 😉)

Copilot Cowork is powerful, but it has boundaries:

- **Cloud-only execution** — it can't touch your local files or run scripts on your machine
- **M365 ecosystem boundary** — without plugins, it stays within Outlook, Teams, SharePoint, and OneDrive
- **Still in preview** — currently available through the Frontier program, not GA yet
- **Custom skills cap** — 50 per user, which sounds like a lot until you start building

For many knowledge workers, these limits won't matter. But for developers, power users, and people who want to move *fast* — customizing beyond what the M365 product team has shipped — there's another path.

## The Coding Agent Path: GitHub Copilot CLI

Enter [GitHub Copilot CLI](https://docs.github.com/en/copilot/github-copilot-in-the-cli/about-github-copilot-in-the-cli) — a terminal-based AI agent that can create and modify files, execute commands, manage Git workflows, and complete multi-step development tasks from your command line. Full MCP server support built in.

Here's the kicker: **WorkIQ offers an MCP server** that plugs directly into Copilot CLI. The same intelligence layer that powers Cowork — your work context, your collaborators, your priorities — now available in your terminal. That means you can build custom skills backed by real organizational context, with no sandbox limitations. Local file access, custom code execution, your own tool integrations — sky's the limit.

![IMAGE: A split-screen terminal and M365 interface illustration — left side shows a dark terminal with code and a glowing AI cursor, right side shows the familiar M365 Copilot chat — both connected by a shared 'WorkIQ' bar at the bottom, emphasizing the common intelligence layer]({{site.baseurl }}/assets/blogs/05122026_pic04.png)

## A Real-World Example: HubWorks

This isn't theoretical. My team at the **Microsoft Innovation Hub** has built exactly this — an end-to-end agentic workflow toolkit called **HubWorks**. It orchestrates our customer engagement lifecycle: from initiating engagements, building agendas, and generating closeout summaries to drafting follow-up emails and producing customer insight reports. All powered by custom skills, MCP servers (including WorkIQ), and GitHub Copilot CLI.

I'll publish a deep-dive on HubWorks in a future post (stay tuned 👀), but the short version: when you combine a coding agent with Skills and WorkIQ, you stop *using* AI tools and start *building with* them. The productivity gains are real and compounding.

## The Architecture: Two Paths, One Vision

Here's how I see the landscape coming together:

<!-- TODO: Insert screenshot of architecture diagram from slides (05122026_pic05.png) -->
![IMAGE: placeholder — use architecture slide from HTML presentation]({{site.baseurl }}/assets/blogs/05122026_pic05.png)

Both paths share the same foundation — large language models, Microsoft Graph, and your M365 data. They share the same intelligence layer in WorkIQ. They even share the same extension model through MCP and Skills. The key difference? The interface layer and how deep you want to customize.

| | SaaS AI (Copilot Cowork) | Coding Agent (GitHub Copilot CLI) |
|---|---|---|
| **Interface** | M365 apps, natural language | Terminal, IDE, scripts |
| **Best for** | Knowledge workers, business users | Developers, power users, makers |
| **Customization** | Custom skills, plugins | Unlimited — custom code, MCP servers |
| **Limitation** | Cloud sandbox, M365 boundary | Requires coding comfort |

## So Which Path Should You Choose?

Honestly? I don't know — and I mean that in the best way possible. Things are moving so fast that today's limitation is next month's feature. But here are some things I'm confident about:

**Both paths are here to stay.** They serve different people with different skill sets and preferences. Your choice comes down to: What's your UI preference? How custom do you need your agent to be? How hands-on do you want to get?

But here's the truly exciting part — **the line between "software maker" and "end user" is getting blurred**, and I love it.

**If you're a developer:** I'd encourage you to spend *less* time thinking about code specifications and *more* time thinking about business process requirements. The AI can write the code. Your superpower is understanding the *problem*.

**If you're a business user:** Try building something. Seriously. Whether it's the agent builder in Copilot, Power Platform, or even GitHub Copilot for "vibe coding" — you'll be amazed at how much you can deliver without writing a single line of code yourself. The tools meet you where you are.

## Welcome to the Era of AI

The future of work isn't about picking a lane between SaaS and code. It's about meeting in the middle — where business understanding meets technical capability, and where AI amplifies both.

So are you excited? Because I am.

Let's embrace work in the era of AI. 🚀

---

*Have thoughts? Find me on [LinkedIn](https://www.linkedin.com/in/luyuanli) or [GitHub](https://github.com/willlimtc). And if you're curious about HubWorks or how our team is putting this into practice — that deep dive is coming soon.*
