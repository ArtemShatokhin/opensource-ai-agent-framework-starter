# Run agents in the cloud on Kortix, the open-source AI Operating System

A session is one unit of agent work. It runs in its own sandbox, on its own git branch, so an agent can install, run and break anything while only what it commits survives. Three things make that safe to hand over: the cloud computer, the branch, and the change request.

## One cloud computer per session

Every session gets its own cloud computer. It is a disposable sandbox that boots from the session's template, or from the platform default image when the project has none. The sandbox clones the repo to `/workspace` and starts the harness, OpenCode or pi. The session status moves from `provisioning` to `running`.

One isolated sandbox per session. Each session has its own isolated machine and branch.

Isolation is what makes parallelism safe. You can run thousands of agents in parallel on one config, each on its own cloud computer, and one session cannot touch another's files. Stop a session and its machine is destroyed; the branch it pushed stays.

## One branch per session

The session cuts its branch from `base_ref`, which defaults to the project's default branch. The branch name is the `session_id`, a UUID with no prefix. The agent commits and pushes to its session branch, and by default a session may push only its own branch. The `session_id`, the branch name, and the session's `sandbox_id` field are the same value.

A branch per session is what keeps parallel agents from overwriting each other. Two sessions on the same project write to two branches and open two change requests, and review decides what merges.

## Work lands as a change request

The agent opens a change request against the project's default branch. A person reads the diff and merges it to `main`. Merge is default-deny for agents: session work reaches `main` only through a change request.

```bash
kortix cr ls          # list change requests
kortix cr diff 1      # read the proposed change
kortix cr merge 1     # merge it
kortix cr request-changes 1 --message "Add a test for the empty case"
```

A session can merge its own change request only when its agent's `kortix_permissions` in `kortix.yaml` includes `project.gitops.merge`. The starter agent has `kortix_permissions: all`, so it can. To require a person, give the agent a `kortix_permissions` list without `project.gitops.merge`. A session can never merge a change request it opened itself.

A trigger starts a session with no person present. Because nobody is there to answer an approval prompt, a trigger session's turn keeps its sandbox for at most 30 minutes, where a person-started turn waits for up to two hours.

## Start the next session

```bash
kortix sessions new --prompt "Reproduce issue #142, fix it, and open a change request"
```

The agent works on its own cloud computer and hands back a change request. Connect a model provider first if the project has none, and set the sandbox template in `kortix.yaml` when you need a specific image.
