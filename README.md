description: Open-source starter for building your own AI agent framework and agent team on Kortix, the open-source AI Operating System.

# A runnable agent-framework starter on Kortix, the open-source AI Operating System

This repository is a runnable starter for building an AI agent framework and an agent team on Kortix, the open-source AI Operating System you own. It walks through the project scaffold, one agent, a skill folder, a cron trigger, and the commands that take the project from your machine to a cloud that runs the agents.

Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector in one git repo you own, with the agents working on real cloud computers.

The companion guide for this starter is [opensourceaiagentframework.com](https://opensourceaiagentframework.com/).

## What `kortix init` scaffolds

The starter project is a git repository. `kortix init` writes it, and it holds:

- `kortix.yaml` at the root, the manifest that declares your agents and what each one may reach.
- `agents/kortix.md`, the starter agent's behavior, plus its governance block in `kortix.yaml`.
- `skills/`, where an agent keeps the know-how it loads only when a task needs it.
- `triggers/`, a cron trigger that starts a session on a schedule with nobody present.
- The runtime config the harness reads, plus the files the next commands push.

## What you own

Kortix is the open-source AI Operating System you own. One git repo holds your agents, the skills they share, your company memory, and every connector. You can read the code, fork it, and audit it.

Every session runs on its own cloud computer, on its own branch, and lands its work through a change request a person reviews. Any model provider with your own keys, or the ChatGPT plan you already pay for. Run it on Kortix Cloud, in your VPC, or on your own on-prem network. Self-host is free.

## Repo file map

| File | What it holds |
| --- | --- |
| `README.md` | What the starter is and the first commands |
| `docs/getting-started.md` | Install the CLI, scaffold, ship, start a session |
| `docs/agents-and-skills.md` | Agents and skills as markdown, with a `kortix.yaml` example |
| `docs/running-agents-in-the-cloud.md` | One cloud computer and one branch per session |
| `docs/faq.md` | Questions developers ask about running agents on Kortix |

## Quickstart

```bash
# 1. Install the CLI
curl -fsSL https://kortix.com/install | bash

# 2. Scaffold a project
kortix init my-agent && cd my-agent

# 3. Ship it
kortix ship
```

Then start a session and review what the agent proposes:

```bash
kortix sessions new --prompt "Summarize this week's commits and open a change request"
kortix cr ls
```

Prefer no setup? Create a project at [Kortix](https://kortix.com) and start a session there.

## Read next

- [Get started with Kortix](docs/getting-started.md)
- [Agents and skills](docs/agents-and-skills.md)
- [Running agents in the cloud](docs/running-agents-in-the-cloud.md)
- [Kortix FAQ](docs/faq.md)
- [Kortix on GitHub](https://github.com/kortix-ai/suna) and the [Kortix docs](https://kortix.com/docs)


## Guides on the companion site

The companion site has a guide for each step: [how to run an agent framework](https://opensourceaiagentframework.com/how-to-run-an-agent-framework) covers the runtime around the agents, [how to build an AI agent](https://opensourceaiagentframework.com/how-to-build-an-ai-agent) walks the build path from install to a merged change request, and [framework or platform](https://opensourceaiagentframework.com/framework-or-platform) explains which layer this starter belongs to.
