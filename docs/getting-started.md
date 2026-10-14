# Get started with Kortix, the open-source AI Operating System

The shortest path from an empty machine to a running agent is install, scaffold, ship, and start a session. Every command below is a real Kortix CLI command, and the same steps work against Kortix Cloud or a self-hosted instance.

## 1. Install the CLI

```bash
curl -fsSL https://kortix.com/install | bash
```

The installer supports macOS and Linux on x86_64 and arm64. On Windows, run it inside WSL. It downloads the release binary for your system, checks it against the release's SHA256SUMS, puts it in `~/.kortix/kortix`, and links `kortix` into `/usr/local/bin`. Check the install with:

```bash
kortix version
```

To install a fixed release instead of the latest, pass it to the installer:

```bash
curl -fsSL https://kortix.com/install | KORTIX_VERSION=0.13.52 bash
```

## 2. Sign in

```bash
kortix login
```

The CLI opens your browser, you approve it there, and the token comes back on a local callback. `kortix login` selects your account and sets a default project for the host. With several accounts it asks, and `--account <slug>` skips the question. On a headless machine, sign in with a token instead:

```bash
kortix login --token <token>
```

## 3. Scaffold a project

```bash
kortix init my-agent && cd my-agent
```

`kortix init` writes the project scaffold: a `kortix.yaml` at the root, plus your agents, skills, and runtime config. It is the version 2 manifest, so the starter agent's behavior lives in `agents/kortix.md` and its governance lives in the `agents.kortix` block of `kortix.yaml`.

## 4. Validate the manifest

```bash
kortix validate
```

`kortix validate` checks `kortix.yaml` against the schema before you push. It exits 0 with no errors and 1 with one or more. `kortix validate --scopes` prints every grantable `kortix_permissions` entry. The same schema check runs in `kortix ship` and in the change request merge check.

## 5. Ship the project

```bash
kortix ship
```

`kortix ship` validates `kortix.yaml`, commits every change, asks for missing secrets, pushes the current branch to the branch with the same name, and asks you to connect declared connectors. The first run also creates the project. `kortix deploy` is the same command.

## 6. Start a session

```bash
kortix sessions new --prompt "Add a skill that drafts the weekly status report"
```

The agent works in its own sandbox, on its own branch. Add `--wait` to return when the sandbox is ready, after at most about five minutes, or `--agent <name>` to run a chosen agent instead of the project's default agent.

## 7. Follow and review

```bash
kortix chat <session_id>                 # a line chat with the agent
kortix sessions wait-for <session_id>    # block until the agent is done
kortix cr ls                             # list change requests
kortix cr diff 1                         # read the proposed change
kortix cr merge 1                        # merge it into main
```

Work reaches the default branch through a change request, and a person merges it. Merge is default-deny for agents. To send the session a revision note instead of merging:

```bash
kortix cr request-changes 1 --message "Add a test for the empty case"
```

## Run the stack on your own infrastructure

```bash
kortix self-host start
```

`kortix self-host start` installs and runs the full Kortix stack on your own infrastructure with one Docker Compose stack. Self-host is free. The [Kortix docs](https://kortix.com/docs) cover the full command surface, hosts, and accounts.

The companion guide for this starter is [opensourceaiagentframework.com](https://opensourceaiagentframework.com/).
