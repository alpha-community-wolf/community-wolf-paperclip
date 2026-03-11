# Community Wolf Setup Todos

This file tracks the next setup and operations tasks for the Community Wolf Paperclip workspace.

## Current Priorities

- [ ] Confirm the CEO fully understands how every other active agent is set up.
- [ ] Document the current external connections available to the company and agents.
- [ ] Confirm the intended use of Gmail and GitHub across the active agents.
- [ ] Define the database access policy for agents.
- [ ] Set up read-only database access for agents via MCP where appropriate.
- [ ] Create the required MCP connections for the relevant Claude agent instances.
- [ ] Decide which agents should receive which MCP connections.
- [ ] Add an inventory of available skills for each active agent.
- [ ] Keep the skill inventory updated as new skills are added under each agent workspace.
- [ ] Review each skill for correct permissions, auto-use behavior, and scope.

## Agent Understanding

- [ ] Alpha should understand the setup of Corey, Tony, Steve, and Mark.
- [ ] Alpha should know each agent's working directory, instruction files, and main responsibilities.
- [ ] Alpha should know which agents use shared project workspaces versus per-agent workspaces.

## Connections

- [ ] Record that Gmail is connected.
- [ ] Record that GitHub is connected.
- [ ] Decide which agents should be allowed to use Gmail.
- [ ] Decide which agents should be allowed to use GitHub.
- [ ] Decide whether any other external services need to be connected now.

## Databases And MCP

- [ ] Define which databases agents should be able to query.
- [ ] Keep all agent database access read-only unless there is an explicit reason not to.
- [ ] Create MCP connections for the required Claude agent environments.
- [ ] Document how each MCP connection should be used.
- [ ] Document any access boundaries or approval rules for MCP-backed tools.

## Skills

- [ ] Create a per-agent skill inventory in this workspace.
- [ ] Record skills stored under each agent's `.agents/skills/` folder.
- [ ] Record what each skill is for, when it should auto-run, and what permissions it needs.
- [ ] Review placeholder skills and replace them with real skills where useful.
- [ ] Add any shared conventions for writing future skills.

## Suggested Next Docs

- [ ] Add a `SKILLS.md` inventory file for the workspace.
- [ ] Add a `CONNECTIONS.md` file if external integrations grow.
- [ ] Add an `MCP.md` file if MCP setup becomes substantial.
