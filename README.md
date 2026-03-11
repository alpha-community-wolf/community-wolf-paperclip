# Community Wolf Paperclip Workspace

This directory is the local control and workspace root for the current Community Wolf company setup.

See also:

- `TODOS.md` - current setup and operations checklist

## Paperclip CLI

Use `npx` to run the Paperclip CLI commands:

```sh
npx paperclipai run
npx paperclipai configure
npx paperclipai doctor
```

These are the main commands for starting Paperclip, updating configuration, and diagnosing setup issues.

## Active Agents

- `agents/alpha` - CEO
- `agents/corey` - Head of Marketing and Brand
- `agents/tony` - Head of Engineering
- `agents/steve` - Founder Social Media Manager
- `agents/mark` - Full Stack Engineer

## Recommended Paperclip Paths

For each agent in Paperclip, set:

- `AGENT_HOME` to that agent's folder under `agents/`
- `cwd` to that agent's root folder under `agents/`
- `instructionsFilePath` to that agent's `AGENTS.md`

If an agent needs Google Workspace access via `gog`, also inject any required `GOG_*` environment variables in that agent's adapter config so the agent can use Gmail/Calendar/Drive non-interactively.

Examples:

- Alpha: `AGENT_HOME=/Users/alpha/Documents/community-wolf-paperclip/agents/alpha`
- Alpha `cwd`: `/Users/alpha/Documents/community-wolf-paperclip/agents/alpha`
- Alpha `instructionsFilePath`: `/Users/alpha/Documents/community-wolf-paperclip/agents/alpha/AGENTS.md`

- Corey: `AGENT_HOME=/Users/alpha/Documents/community-wolf-paperclip/agents/corey`
- Corey `cwd`: `/Users/alpha/Documents/community-wolf-paperclip/agents/corey`
- Corey `instructionsFilePath`: `/Users/alpha/Documents/community-wolf-paperclip/agents/corey/AGENTS.md`

- Tony: `AGENT_HOME=/Users/alpha/Documents/community-wolf-paperclip/agents/tony`
- Tony `cwd`: `/Users/alpha/Documents/community-wolf-paperclip/agents/tony`
- Tony `instructionsFilePath`: `/Users/alpha/Documents/community-wolf-paperclip/agents/tony/AGENTS.md`

- Steve: `AGENT_HOME=/Users/alpha/Documents/community-wolf-paperclip/agents/steve`
- Steve `cwd`: `/Users/alpha/Documents/community-wolf-paperclip/agents/steve`
- Steve `instructionsFilePath`: `/Users/alpha/Documents/community-wolf-paperclip/agents/steve/AGENTS.md`

- Mark: `AGENT_HOME=/Users/alpha/Documents/community-wolf-paperclip/agents/mark`
- Mark `cwd`: `/Users/alpha/Documents/community-wolf-paperclip/agents/mark`
- Mark `instructionsFilePath`: `/Users/alpha/Documents/community-wolf-paperclip/agents/mark/AGENTS.md`

## Standard Agent Folder

Each agent should follow this layout:

```text
agents/<agent-name>/
  AGENTS.md
  SOUL.md
  HEARTBEAT.md
  TOOLS.md
  .agents/
    skills/
  workspace/
  memory/
  life/
```

Guidelines:

- The agent root is the runtime working directory.
- `.agents/` lives at the agent root because that is where the agent runtime starts.
- All real project work, repo work, drafts, and generated outputs should go inside `workspace/`.
- `memory/` and `life/` hold longer-lived agent notes and personal context.

## SOP: Add A New Agent

1. Create a new folder under `agents/<agent-name>/`.
2. Add `AGENTS.md`, `SOUL.md`, `HEARTBEAT.md`, and `TOOLS.md`.
3. In `AGENTS.md`, include the safety rule: **Never exfiltrate secrets or private data.**
4. Create the standard subfolders: `.agents/skills/`, `workspace/`, `memory/`, and `life/`.
4. In Paperclip, set `AGENT_HOME` to the new agent folder.
5. In Paperclip, set `cwd` to the new agent folder root.
6. In Paperclip, set `instructionsFilePath` to the new agent's `AGENTS.md`.
7. Put all actual working files and repositories inside `workspace/`.
8. If the agent needs shared integrations, add them to the `External Apps And Connections` section in that agent's `AGENTS.md`.
9. If the agent needs local skills, add them under `.agents/skills/<skill-name>/SKILL.md`.

## Projects

- `projects/core-product` - engineering workstream
- `projects/brand-growth` - marketing and founder-brand workstream

## Shared Context

- `company/` - durable company context shared across agents
- `company/brand/` - stable brand guidance and messaging references
- `company/founders/` - founder voice, bios, and founder-specific context
- `company/product/` - product overview and product facts
- `company/audience/` - audience notes, personas, and market context
- `company/references/` - long-lived supporting reference material

## Practical Rules

- Put durable company knowledge in `company/`.
- Put active workstream documents in `projects/<project-name>/`.
- Put agent-specific scratch work, drafts, and temporary files in `agents/<name>/workspace/`.
- If a document should still matter across multiple projects in a few months, it belongs in `company/`.
- If a document is specific to one current campaign, launch, or initiative, it belongs in that project folder.
- If a file is mainly useful to one agent while they are actively working, it belongs in that agent's own workspace.

## Goal Levels

- `company` - the highest-level outcome for the business or a major function. Use this for things like overall brand presence, revenue, product traction, or company-wide strategic priorities.
- `team` - a department or workstream outcome that supports a company goal. Use this for goals owned by marketing, engineering, founder brand, or another team-level effort.
- `agent` - an outcome owned by one specific agent. Use this when one person should clearly own the result, for example Michael thought leadership or Mark shipping a particular capability area.
- `task` - the smallest goal level, tied to a concrete deliverable or near-term piece of work. Use this when the goal is effectively the outcome of a specific task or short execution cycle.

## Goal Rules Of Thumb

- Use `company` when the goal should matter to leadership and remain stable for a while.
- Use `team` when multiple tasks or agents contribute to one functional outcome.
- Use `agent` when one agent is the clear owner of the result.
- Use `task` when the outcome is narrow, immediate, and execution-specific.
- If you are unsure, start higher-level and only split into lower levels when ownership or measurement becomes unclear.

## Where Config Lives

- Agent adapter config -> saved in the Paperclip database. This is where agent runtime settings like adapter type and adapter config are stored.
- Instructions, skills, and workspace docs -> saved as files in this workspace. Use the `agents/`, `company/`, and `projects/` folders for those.
- Paperclip app config -> `~/.paperclip/instances/default/config.json`. This is instance-level Paperclip configuration, not the per-agent runtime config from the UI.

## Notes

- Older role-based folders were left in place so you can migrate safely.
- For engineering agents, clone each required repository into the agent's own `workspace/` folder.
- If two engineering agents need to work in parallel, give each agent its own clone set and branch history.
