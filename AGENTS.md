# AGENTS.md

## Scope

This file defines the startup contract for agents working from `clever-context-monorepo`.

This repository is part of the CLEVER three-repository local control plane.
It is the **canonical interpretation repository**, not the default intake entrypoint.

## Core Rule

Use this distinction first:

- If this session is **generic CLEVER work startup**, do not start here. Move startup to `clever-agent-project`.
- If this session explicitly **edits `clever-context-monorepo` itself**, stay here and treat this repository as the current target.

Short version:

- generic startup: redirect to `clever-agent-project`
- repo-local maintenance: stay in `clever-context-monorepo`

## Workspace Contract

CLEVER expects a local three-repository workspace:

1. `clever-agent-project`
2. `clever-context-monorepo`
3. `clever-change-control`

Before doing real work, run the automatic workspace check from the current session:

```bash
python3 ../clever-agent-project/scripts/bootstrap_clever_work.py --cwd "$PWD" --workspace-check --json
```

If the session is explicitly about editing this repository itself, run:

```bash
python3 ../clever-agent-project/scripts/bootstrap_clever_work.py --cwd "$PWD" --workspace-check --current-repo-maintenance --json
```

Interpret `agent_action` like this:

- `proceed-with-hard-gate`: the workspace is complete and startup may continue
- `current-repo-maintenance`: this repository itself is the target, so stay here
- `switch-to-clever-agent-project`: this is generic startup work, so move to `clever-agent-project`
- `stop-and-fix-workspace`: the local workspace is incomplete, so do not continue as if startup were healthy

## What This Repository Owns

Use this repository for:

- root rules and authority boundaries
- template lineage and deploy baseline
- service metadata
- contracts and interpretation context

## Main Branch Contract

This repository is public and its `main` branch is protected by the GitHub
ruleset `CLEVER protect main`.

- Do not push directly to `main`.
- Use a role-prefixed branch for repository changes.
- Open a PR into `main`.
- `main` requires a PR but does not require approving reviews.
- Merge/write access is limited to repository admins.
- Admin bypass is allowed only in `pull_request` mode.

Do not treat this repository as:

- the default generic startup surface
- the implementation repository for product code
- the rollout or rollback ledger

## Startup Behavior

If the user is starting a new CLEVER task and the target repo is not yet fixed:

1. Run the workspace check
2. If the workspace is healthy, move startup to `clever-agent-project`
3. Apply the first-response hard gate there

If the user is directly changing this repository:

1. Stay in the current repo
2. Treat `clever-context-monorepo` as the target repo for this session
3. Still respect the wider CLEVER authority model when referencing sibling repos

## Target Repository Traceability Gate

When this repository is opened as part of a CLEVER session for another target
project, the agent must follow the target-repository traceability gate defined in
`../clever-agent-project/AGENTS.md`.

That gate applies regardless of the current working directory. Before target
implementation begins, the agent must anchor the work in `clever-change-control`,
cross-mention the root context issue, the `clever-change-control` issue, and the
target repository issue, then create or confirm an issue-based branch.

Do not treat GitHub automatic issue references as enough. GitHub may link plain
mentions, but the agent is responsible for maintaining the working context:
session phase, target repository, branch name, commits, PR link, current status,
and next action on the `clever-change-control` issue.

Use `fixes`, `closes`, or similar GitHub keywords only when the PR merge is
intended to close the referenced issue. Use plain issue mentions for context
links.

## Read Order For Repo-Local Work

When the task is specifically about this repository, read in this order:

1. `README.md`
2. `docs/root/authority-boundaries.md`
3. `docs/root/index.md`
4. the specific `docs/root`, `docs/services`, or `docs/templates` files touched by the task
5. `../clever-agent-project/AGENTS.md`
6. `../clever-change-control/README.md`

## Expected Behavior

Do not automatically redirect every session away from this repository.

Redirect only when the session is generic startup.
Stay here when the work is explicitly about modifying `clever-context-monorepo` itself.
