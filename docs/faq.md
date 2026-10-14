# Kortix FAQ: the open-source AI Operating System for agent frameworks

Answers to the questions developers ask when they build and run an agent framework on Kortix.

## What is Kortix, in one sentence?

Kortix is the open-source AI Operating System: your agents, their skills, your company memory, and every connector in one git repo you own, with the agents working on real cloud computers. It is an open-source alternative to Claude Cowork and ChatGPT Work, and you run it on Kortix Cloud, in your VPC, or on your own on-prem network.

## Do I need a server to run the agents?

No. Every session gets its own cloud computer, an isolated sandbox on its own branch. The sandbox clones your repo and runs the agent there, and one session cannot touch another's files. Because each session is isolated, you can run thousands of agents in parallel on one config. Self-host is free if you would rather run the stack on your own infrastructure.

## Can I use my own model and API keys?

Yes. Any model provider with your own keys. Or the ChatGPT plan you already pay for. Or sign in with your OpenCode Console account for OpenCode Zen and Go. You set the model in the project configuration, so switching providers is a config change, not a migration.

## How do I write an agent or a skill?

An agent is a markdown file, `agents/<name>.md`, plus a governance block in `kortix.yaml`. A skill is a folder, `skills/<name>/SKILL.md`, with a `description` that tells the agent when to load it. Both live in the same git repo as the rest of the project, so you version and review them like code.

## Who reviews what an agent changes?

A person does. A session works on its own branch and opens a change request; a person reads the diff and merges it to `main`. Merge is default-deny for agents, so nothing reaches the default branch without a change request. A starter agent with `kortix_permissions: all` can merge its own change request unless you remove `project.gitops.merge` from its grants.

## How much does Kortix cost?

Free includes 200 credits each month for sandbox compute and 1 project. The team plan is $40 per seat per month and includes 2,500 pooled credits per seat. Self-host is free. Plan details change, so read the current plans at [Kortix](https://kortix.com/pricing).

## More resources

The [Kortix docs](https://kortix.com/docs) list every command, host and grant, and [getting-started.md](getting-started.md) walks the first four commands.
