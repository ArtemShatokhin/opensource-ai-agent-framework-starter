# Agents and skills on Kortix, the open-source AI Operating System

An agent is a system prompt, a model and tool rules that a session runs, plus grants that decide which connectors, secrets, skills, Apps and Kortix permissions it may use. Both halves live in the project repo, and the repo is what you version and review. The files come first, then a `kortix.yaml` that wires them together.

## An agent is a markdown file plus a manifest block

An agent is `agents/<name>.md` for its behavior, plus an `agents.<name>` block in `kortix.yaml` for its governance. The `.md` file is a stock OpenCode agent file: YAML frontmatter, then a body that is the system prompt. Governance always lives in `kortix.yaml`; in a version 2 manifest, behavior lives in the agent file.

`agents/support.md`:

```markdown
---
description: Answers support tickets from the help desk.
mode: primary
permission:
  bash: ask
---

You answer support tickets. Read the ticket, check the order history, draft a reply, and submit it for review. Never close a ticket yourself.
```

The scoping field is `permission`, not `tools`. `tools:` and `maxSteps:` are both errors, and the message names the replacement. Every frontmatter field is optional, so a stock OpenCode agent file is valid as written. The `mode` field takes `primary`, `subagent` or `all`.

## Governance in kortix.yaml

The `agents.<name>` block decides what the agent may reach. In a version 2 manifest, put only governance keys in this block. A behavior key such as `model`, `mode`, `prompt` or `permission` here is an error; it belongs in the agent file.

```yaml
# yaml-language-server: $schema=https://kortix.com/schema/kortix.v2.schema.json
kortix_version: 2
default_agent: kortix

agents:
  kortix:
    file: agents/kortix.md
    connectors: all
    secrets: all
    skills: all
    kortix_permissions: all

imports:
  - triggers/
```

Each grant does one job:

- `connectors` lists the app, API or MCP slugs the agent may call. `all` grants every connector the project declares.
- `secrets` lists the project secret names the agent may receive. Secret values never appear in the repo.
- `skills` lists the skills the agent may load. `all` grants every skill on the path; a list grants the named skills and denies the rest.
- `kortix_permissions` lists the platform grants. `all` includes `project.gitops.merge`, the merge capability. Give the agent a list without `project.gitops.merge` when a person must merge every change it opens.

An agent name matches `[a-z0-9][a-z0-9_-]{0,127}`. The manifest declares at least one agent and sets `default_agent`. A grant you omit means none.

## A skill is a folder

A skill is a folder of instructions an agent loads when a task needs it. Kortix does not put skill bodies in every prompt, so a skill keeps a long procedure out of the agent's context until it is relevant. The file is `skills/<name>/SKILL.md`, and the `description` is what the agent reads to decide when to load it.

```markdown
---
name: triage
description: Classify a support ticket by product area and urgency. Use for every new ticket.
---

# Triage

1. Read the ticket and the customer's last three tickets.
2. Tag the product area and the urgency.
3. Draft a one-line summary for the queue.
```

Write the description as when to use the skill, not only what it is. Reference files can sit beside `SKILL.md` in the same folder.

## Triggers live in the same repo

A trigger starts a session with nobody present, on a schedule or from a signed webhook. A trigger lives under `triggers:` in `kortix.yaml`, or in a file listed under `imports:`. This file, `triggers/daily-digest.yaml`, is imported by the manifest above:

```yaml
triggers:
  - slug: daily-digest
    type: cron
    agent: kortix
    cron: "0 0 9 * * 1-5"
    timezone: Europe/Berlin
    prompt: Summarize yesterday's commits and open change requests.
```

A cron trigger takes a six-field expression: second, minute, hour, day of month, month, weekday. A trigger fires at most once a minute. An imported file declares `triggers`, `connectors`, `agents`, `apps`, `tools` and `imports`; `kortix_version`, `default_agent`, `env`, `sandbox` and `policy` belong in the root manifest only. Do not add a `channels:` block: a version 2 manifest rejects it.

## Validate before you push

```bash
kortix validate
kortix validate --scopes
```

`kortix validate` catches a missing agent file, a skill with no `SKILL.md`, an undeclared connector slug and an undeclared secret name. An error blocks the push or the merge; a warning does not.
